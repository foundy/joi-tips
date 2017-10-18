# Joi Validator

Object validator인 Joi를 정리해보자.

## Tips

#### reach를 통해 하위 스키마 가져오기

포괄적인 스키마를 구성한 후 reach를 통해 필요한 하위 스키마만 가져와서 사용하기 편하다.  
그리고 reach를 통해 가져온 하위 스키마 구성을 변경해도 기존 스키마는 그대로 유지된다.

```javascript
const schema = Joi.object({
  foo: Joi.object({
    first: Joi.string(),
    second: Joi.any(),
  }),
  bar: Joi.number(),
});
const firstSchema = Joi.reach(schema, 'foo.first').trim().required();

console.log('schema', Joi.validate({ foo: { first: '  a  ' } }, schema));
// schema { error: null, value: { foo: { first: '  a  ' } } }
console.log('firstSchema', Joi.validate('  a  ', firstSchema));
// firstSchema { error: null, value: 'a' }
```

#### object의 keys 초기화

Object 키를 정의하는 경우 해당 키가 존재하면 재정의하고, 없으면 추가된다.  
전체 키를 초기화하고 싶은 경우는 아래와 같이 사용 할 수 있다.

```javascript
const schema = Joi.object({
  foo: Joi.string().valid('poo'),
  bar: Joi.number().min(2),
});

// Original schema error! child "foo" fails because ["foo" must be one of [poo]
Joi.validate({ foo: 'FOO', bar: 1 }, schema, (error, value) => {
  if (error) {
    console.log('Original schema error!', error.message);
  }
});

const cleanKeys = schema.keys();

// No error output
Joi.validate({ foo: 'FOO', bar: 1 }, cleanKeys, (error, value) => {
  if (error) {
    console.log('Clean schema error!', error.message);
  }
});
```

## Reference

- [Celebrate](https://github.com/continuationlabs/celebrate)
  - Express 프레임워크 기반으로는 Celebrate가 그나마 다운로드 수가 가장 많다.
  - NPM Stats (2017.10.18)  
    *292 downloads in the last day*  
    *1,676 downloads in the last week*  
    *7,883 downloads in the last month*