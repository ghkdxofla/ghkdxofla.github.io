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