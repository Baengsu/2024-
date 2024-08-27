import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns  # 히트맵을 그리기 위해 seaborn 추가

# 데이터 로드
df_fire = pd.read_csv('일일산불데이터_산림청.csv')

# 데이터 전처리
df_fire = df_fire.drop(index=0)
df_fire.columns = df_fire.iloc[0]
df_fire = df_fire.drop(index=1)
df_fire = df_fire.reset_index(drop=True)

# 충남 지역 필터링 및 불필요한 열 제거
df_cn = df_fire[df_fire['발생장소_관서']=='충남']
df_cn = df_cn.drop(columns=['발생원인_구분','발생장소_시도','발생원인_세부원인', '진화종료시간_년', '진화종료시간_월', '진화종료시간_일','진화종료시간_시간'])
df_cn = df_cn.reset_index(drop=True)

# 데이터 타입 변환
df_cn = df_cn.astype({
    '발생일시_년': int,
    '발생일시_월': int,
    '발생일시_일': int,
    '피해면적_합계' : float
})

# 2014~2023 충남 시군구별 화재 발생 횟수 시각화
counts = df_cn['발생장소_시군구'].value_counts()
counts = counts.sort_values(ascending=False)

plt.figure(figsize=(10, 6))
counts.plot(kind='bar')
plt.title('2014~2023 충남 시군구별 화재 발생 횟수')
plt.xlabel('시군구')
plt.ylabel('횟수')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()

# 연도 별 충청남도 산불 발생 횟수 시각화
count_per_year_sigungu = df_cn.groupby(['발생일시_년', '발생장소_시군구']).size().unstack().fillna(0)

sorted_years = sorted(count_per_year_sigungu.index)
count_per_year_sigungu = count_per_year_sigungu.loc[sorted_years]

# 컬러맵 설정
colormap = plt.get_cmap('tab20')  
colors = [colormap(i) for i in range(len(count_per_year_sigungu.columns))]

# 막대그래프 시각화
ax = count_per_year_sigungu.plot(kind='bar', stacked=True, figsize=(14, 8), color=colors)

# 연도 별 총 발생 횟수 표시
for i, (year, row) in enumerate(count_per_year_sigungu.iterrows()):
    total_count = row.sum()
    ax.text(i, total_count + 0.5, int(total_count), ha='center', va='bottom', fontsize=10, color='black')

plt.title('연도 별 충청남도 산불 발생 횟수')
plt.xlabel('발생연도')
plt.ylabel('발생횟수')
plt.legend(title='시군구', bbox_to_anchor=(1.05, 1), loc='upper left')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# 연도 별 행정 구역 산불 발생 횟수 히트맵 시각화
count_per_year_sigungu = df_cn.groupby(['발생일시_년', '발생장소_시군구']).size().unstack().fillna(0)
count_per_year_sigungu = count_per_year_sigungu.T.sort_index(ascending=False)

plt.figure(figsize=(14, 8))
sns.heatmap(count_per_year_sigungu, cmap="YlGnBu", annot=True, fmt=".0f")
plt.title('연도 별 행정 구역 산불 발생 횟수')
plt.xlabel('발생년도')
plt.ylabel('발생지역')
plt.show()
