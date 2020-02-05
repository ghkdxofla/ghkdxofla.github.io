# table에서 tbody 안에 세로 스크롤이 있는 표 만들기
```css
table ,tr td{
    border:1px solid red
}

tbody {
    display:block;
    height:50px;
    overflow:auto;
}

thead, tbody tr {
    display:table;
    width:100%;
    table-layout:fixed;/* even columns width , fix width of table too*/
}

thead {
    width: calc( 100% - 1em )/* scrollbar is average 1em/16px width, remove it from thead width */
}

table {
    width:400px;
}

/* 안되면 아래의 방법도 해보기 */
.TABLE{border-collapse:collapse; width:100%}
.TABLE thead{float:left; width:890px;}
.TABLE tbody{overflow-y:auto; overflow-x:hidden; float:left; width:890px; height:100px}
.TABLE tbody tr{display:table; width:890px;}
.TABLE td{width:100px}
```

# 참조 링크
