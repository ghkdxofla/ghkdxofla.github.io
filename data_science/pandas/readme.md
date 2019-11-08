# Pandas

Module Import
```
import pandas as pd
```
기본적인 Dataframe 형성 및 출력(from MSSQL record 객체)
```
# Dataframe 형성
mssql_data = get_record() # Record를 가져오는 것으로 가정
df = pd.DataFrame.from_records(mssql_data, columns=['id', 'pwd']) # Column을 잘 맞추는게 중요

# Datafrmae 출력(n -> 항목 개수))
df.tail(n=10)
```
Column 목록 가져오기
```
columns = [column[0] for column in cursor.description]
```
Count 후, 해당 항목 Column에 추가하기
```
df.groupby(['A','B']).B.agg('count').to_frame('c').reset_index()
#df.groupby(['A','B']).size().to_frame('c').reset_index()

Out[593]: 
   A  B  c
0  x  p  2
1  y  q  1
2  z  r  2
```