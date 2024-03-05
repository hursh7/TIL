# React-Hook-Form

리렌더링 이슈를 해결하고자 `ref` 을 이용하는 비제어 컴포넌트방식의 form 관리 라이브러리.

### React-Hook-Form 프로젝트 적용 예시 (lge.com_admin)

**`React-Hook-Form` 적용 전 `Custom-Hook` 방식의 코드**

custom hook 인 `useInput` 과 `useValidation` 을 사용해서 상태를 관리하고 유효성 검사를 하는 
회원 가입 form 컴포넌트

```jsx
import { useInput, useValidation } from '@hooks';

  const [id, setId, onChangeId] = useInput('');
  const [password, setPassword, onChangePassword] = useInput('');
  const [email, setEmail, onChangeEmail] = useInput('');

	const idValidate = useValidation('id', id);
  const emailValidate = useValidation('email', email);
  const passwordValidate = useValidation('password', password, id);

	return (
	  <Input
      type='text'
      value={id.replace(/[^a-z0-9]/g, '')}
      name={id}
      onChange={onChangeId}
      confirmedMessage={idValidate.isBoolean}
      />
		 <Input
      type='text'
      value={password}
      name={password}
      onChange={onChangePassword}
      confirmedMessage={passwordValidate.isBoolean}
      />
		 <Input
      type='text'
      value={email}
      name={email}
      onChange={onChangeEmail}
      confirmedMessage={emailValidate .isBoolean}
      />
			...
	)
```

⚠️ 각각의 id, password, email 상태 중 하나라도 변경되면 리렌더링이 발생한다.

⚠️ 유효성 검사를 위한 값들을 따로 관리해줘야 하며 추가적으로 복잡한 로직을 추가 해줘야 하는 경우가 빈번히 생긴다.

**`React-Hook-Form` 적용 코드**

✅ 각각의 상태가 변경될 때, 불필요한 리렌더링이 발생하지 않는다.

✅ React-Hook-Form 이 제공하는 `handleSubmit` 함수를 사용하면 submit 과 유효성 검사 처리를 

쉽게 구현하고 관리하기 용이하다.

```jsx

const {
    register,
    handleSubmit,
    watch,
    reset,
  } = useForm({
    defaultValues: {
			id: '',
			password: '',
			email: '',
		},
    mode: 'onChange',
  });

	// 예시: 회원 가입 submit 함수
  const onSignUpSubmit = (data) => {
		// register로 등록되고 useForm에 의해 관리되는 데이터를 인자로 받아온다.
    const { id, password, email} = data;
		... // 회원 가입 API 호출 로직
  );
	
	// 예시: 회원 가입 유효성 검사 함수
  const onFail = (data) => {
    const { id, password, email} = data;
    if (
      id?.type === 'required' ||
      password?.type === 'required' ||
      email?.type === 'required'
    ) {
			// required 상태인 HFInput의 값이 없을 때, 얼럿 출력 
      dispatch(openModalAlert({ message: '올바른 전화번호를 입력해 주세요.' }));
    }
  }

	return (
    <HFInput
      type={'text'}
      name={'id'}
      register={register}
      required
    />
		<HFInput
      type={'text'}
      name={'password'}
      register={register}
      required
    />
		<HFInput
      type={'text'}
      name={'email'}
      register={register}
      required
    />
		// react-hook-form 에서 제공하는 handleSubmit으로 useForm에 의해 관리되는 데이터에 대한
		// submit과 유효성 검사 처리를 쉽게 할 수 있다. 
		<Button onClick={handleSubmit(onSignUpSubmit, onFail)}>회원 가입</Button>
			...
	)
```

### React-Hook-Form 제공 API

```jsx

const HFInput = () => {

	const {
	  register,
	  handleSubmit,
	  watch,
	  reset,
	  setError,
	  setValue,
	  control,
	  formState: { errors },
	} = useForm({
	  defaultValues: AlarmModifiedInterface,
	  mode: 'onChange',
		// onChange | onBlur | onSubmit | onTouched | all
	});

	return (
			<input
			  type={type}
			  className={cn(
			    'input',
			    {
			      required,
			      disabled,
			    },
			    className
			  )}
			  {...register(name, { pattern, required, disabled })}
			  {...rest}
			/>
		)  
};
```

- `getValues` : 호출하는 시점의 값을 가져온다.
- `watch` : observer pattern을 사용하여 element를 구독해서 상태 변화를 감지한다.
    - **watch는 호출될 때 어플리케이션 또는 Form을 전부 리렌더링하므로 성능 이슈가 발생한다면 별도의 콜백함수 또는 useWatch를 사용하라고 공식문서에 명시되어 있다.**
- `useWatch` : **ref**의 변화를 감지해서 watch와 같은 역할을 수행하는데, useWatch 훅이 자체적으로 state를 가지고 해당 값을 반환해주는 역할을 해서 전체 리렌더링을 발생 시키지 않는다.
- `setValue` : setValue("필드명", 값) 으로 사용하여 해당 필드명의 값을 업데이트 해준다.
- `reset` : reset({key1 : value1, key2 : value2, key3 : key3}) 형태로 여러 필드의 값을 업데이트 해준다.
- `Controller` : react-select, AntD, Material-Ui 등의 **제어형 컴포넌트와 사용될 것을 염두에 두고** 만들어진 컴포넌트이다.
    
    > `name` : 가리킬 Form의 field 명
    `control` : useForm의 control
    `render` : field에 의존하는 children Node
    > 
- `useController` : Controller는 useController를 렌더링 하는 것이다.
lge.com_admin 프로젝트에서는 `HFSelectbox` 컴포넌트에 사용되고 있다.
    
    ```jsx
    const { field } = useController({
     name:"userName",
     control:methods.control
    })
    
    const {name, value, onChange, onBlur, ref, formState, fieldState} = field;
    ```
    
- `useFieldArray`
    - 필드에 배열 타입의 객체가 있을 때 사용해 코드의 양을 줄일 수 있다.
    - **useFieldArray는 각 배열 요소에 대해 id를 자체적으로 생성해버린다.**

```jsx
const {fields,append,prepend,remove,swap,move,insert}= useFieldArray({
  control,
  name:'userArray'
})

return(
  <>
      {fields.map((item,index)=>(
      	<Controller
          key={item.id}
          name=`userArray.${index}`
          control={control}
          render={({field})=>(
            	<input onChange={field.onChange} value={field.value}/>
          )}
        />
      )}
  </>
)
```

- `useFormContext`
    
    ```jsx
    const methods = useForm({
    	defaultValues:{
        	key: "value"
        }
    });
    
    return (
     <FormProvider {...methods}><ChildComponent/></FormProvider>
    ```
    

### React-Hook-Form + Yup(에러처리 라이브러리)

`npm install react-hook-form yup @hookform/resolvers`

```jsx
import { useForm } from "react-hook-form";
import { yupResolver } from "@hookform/resolvers/yup";
import * as yup from "yup";

const {
    register,
    handleSubmit,
    formState: { errors }, // 버전 6라면 errors라고 작성함.
  } = useForm({
    resolver: yupResolver(schema),
  });
```

**사용 예시**

폼의 유효성을 검사하는 yup에서 schema를 설정한다.

각각의 input의 name 속성에 지정한 값을 그대로 입력하고, string인지 number인지, 최소 및 최대 글자 수 등 검사 해야 할 사항을 입력한 뒤에 제출 전 꼭 입력 해야 는 사항이면 `required( )`를 마지막에 넣어준다.

`confirmPassword`는 앞에 작성한 password와 다를 경우에 에러 메시지가 나타날 수 있도록 `ref( )`를 지정한다.

```jsx
const schema = yup.object().shape({
  name: yup.string().required("이름을 입력해주세요."),
  email: yup.string().email().required("email을 입력해주세요"),
  password: yup
    .string()
    .min(8)
    .max(15)
    .required("비밀번호는 8 에서 15 글자여야 합니다."),
	// passworddhk 와 다를 경우 에러 메시지 나타나게 ref 지정
  confirmPassword: yup.string().oneOf([yup.ref("password"), null]),
});

<input type="password" name="confirmPassword" 
{...register("confirmPassword")} />
<p>{errors.confirmPassword && "confirm your password"}</p>
```

[npm: yup](https://www.npmjs.com/package/yup)

[Yup 라이브러리 파헤치기](https://velog.io/@boyeon_jeong/Yup-라이브러리-파헤치기)

📚 **참고자료**

[API Documentation](https://react-hook-form.com/docs)

**제어형 컴포넌트 vs 비제어 컴포넌트**

[React: 제어 컴포넌트와 비제어 컴포넌트의 차이점](https://velog.io/@yukyung/React-제어-컴포넌트와-비제어-컴포넌트의-차이점-톺아보기)