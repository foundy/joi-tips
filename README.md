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

## Reference

- [Celebrate](https://github.com/continuationlabs/celebrate)
  - Express 프레임워크 기반으로는 Celebrate가 그나마 다운로드 수가 가장 많다.
  - NPM Stats (2017.10.18)  
    *292 downloads in the last day*  
    *1,676 downloads in the last week*  
    *7,883 downloads in the last month*