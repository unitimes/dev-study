#CSS tips
###Styling `<input type='file'>`
- input element의 visibility를 hidden으로
- `<input type='file'>`을 활성화할 버튼 등을 만든다.
- 활성화 버튼 클릭 시 활성화
```javascript
$('input[type=file]').trigger('click');
```
