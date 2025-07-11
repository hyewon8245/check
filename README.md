# check

# 회고록

# 📊 StackOverflow 설문 데이터 분석 (최근 3개년)

## ✅ 데이터 수집 및 병합

- 2022, 2023, 2024년 설문 결과 CSV 파일을 수집하고, `Age`, `Country`, `CompTotal`, `LanguageHaveWorkedWith`, `DatabaseHaveWorkedWith`, `RemoteWork`, `MainBranch`, `DevType` 컬럼만 선택
- `year` 컬럼을 추가한 후 `pd.concat()`으로 병합
- 병합된 데이터에 대해 `user_id`를 부여하여 고유 식별자 설정

```python
# 병합 및 ID 부여
merged_df = pd.concat([df_2022, df_2023, df_2024], ignore_index=True)
merged_df.insert(0, 'user_id', range(1, len(merged_df) + 1))

```

---

## ✅ 제1정규화: 1:N 관계 분리

- `DevType`, `LanguageHaveWorkedWith`, `DatabaseHaveWorkedWith` 컬럼은 `;`로 구분된 다중값이 존재
- 이를 `explode()`로 분해하여 각각 정규화된 테이블(`use_devtype`, `use_language`, `use_database`)로 분리

```python
# 예시: Language 정규화
lang_df = df[['user_id', 'LanguageHaveWorkedWith']].dropna()
lang_df['LanguageHaveWorkedWith'] = lang_df['LanguageHaveWorkedWith'].str.split(';')
lang_df = lang_df.explode('LanguageHaveWorkedWith')
lang_df['LanguageHaveWorkedWith'] = lang_df['LanguageHaveWorkedWith'].str.strip()
lang_df = lang_df.rename(columns={'LanguageHaveWorkedWith': 'language'})

```

---

## ✅ 주요 통계 분석 결과

### 💾 가장 많이 사용된 데이터베이스 (최근 3개년)

```sql
SELECT database, COUNT(*) AS count
FROM use_database
GROUP BY database
ORDER BY count DESC;

```

📊 결과 시각화:

![image.png](https://chatgpt.com/g/g-p-686dcf0e552c8191aa6d3baa3a9cb2b1-hwaginyong/c/image%201.png)

---

### 🧑‍💻 가장 많이 사용된 프로그래밍 언어

```sql
SELECT language, COUNT(*) AS count
FROM use_language
GROUP BY language
ORDER BY count DESC;

```

📊 결과 시각화:

![image.png](https://chatgpt.com/g/g-p-686dcf0e552c8191aa6d3baa3a9cb2b1-hwaginyong/c/image%202.png)

---

### 👩‍💻 가장 많이 선택된 개발자 유형

```sql
SELECT devtype, COUNT(*) AS count
FROM use_devtype
GROUP BY devtype
ORDER BY count DESC;

```

📊 결과 시각화:

![image.png](https://chatgpt.com/g/g-p-686dcf0e552c8191aa6d3baa3a9cb2b1-hwaginyong/c/image%203.png)

---

## ✅ 회고

- `;`로 나뉜 다중 응답 컬럼은 정규화를 통해 1:N 관계 테이블로 분리
- 데이터 정제 후 SQL을 통해 단순 빈도 분석을 수행했으며, 연도별로도 확장 가능
- 향후에는 연도별 변화 추이 분석, 국가별 연봉 비교 등 추가 분석 가능

---

필요하시면 이 정리 내용을 마크다운 파일로 다시 저장해 드릴 수도 있고, PDF로 내보내는 것도 도와드릴게요. 원하시나요?
