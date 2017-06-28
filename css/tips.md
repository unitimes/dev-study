# CSS tips
### Styling `<input type='file'>`
- `<input type='file'>`은 CSS로 스타일을 변경하는데 제약이 있음
- `<input type='file'>`의 visibility를 hidden으로 변경한 후 기능을 대신할 스타일 가능한 element를 생성
- 스타일 가능한 element에서 event 발생 시 아래 javascript 코드 실행
```javascript
$('input[type=file]').trigger('click');
```
