# Topic: ความนิยมของภาพยนตร์และซีรีส์ใน Netflix

# คำถามที่สนใจ

1. รายการหมวดไหนน่าสนใจที่สุด

2. ในแต่ละหมวดภาพยนตร์และซีรีส์เรื่องไหนเป็นที่นิยมมากที่สุด

3. ประเทศใดผลิตภาพยนตร์และซีรีส์ออกมาแล้วเป็นที่นิยมมากที่สุด

4. ภาพยนตร์หรือซีรีส์มีความนิยมมากกว่ากัน

5. ความนิยมของนักแสดงหรือผู้กำกับมีผลกับความนิยมหรือไม่

6. ภาพยนตร์และซีรีส์ปล่อยออกในปีไหนที่ Netflix เลือกมาเข้ามาฉายเยอะที่สุด

# 1. Data
เป็นข้อมูลภาพยนตร์และซีรีส์ทั้งหมดที่มีใน Netflix เป็นข้อมูล ณ เดือนพฤษภาคม ปี 2022
ข้อมูลที่ใช้ได้มาจาก [https://www.kaggle.com/datasets/thedevastator/the-ultimate-netflix-tv-shows-and-movies-dataset?select=raw_titles.csv](https://www.kaggle.com/datasets/thedevastator/the-ultimate-netflix-tv-shows-and-movies-dataset?select=raw_titles.csv)


ข้อมูลไฟล์ที่ 1 คือ raw_titles.csv มีรายละเอียด ดังนี้
 - id : id ของภาพยนตร์และซีรีส์แต่ละเรื่อง
 - title : ชื่อเรื่อง
 - type : ประเภทมี 2 ประเภท คือ ภาพยนตร์ และ ซีรีส์
 - release_year : ปีที่ภาพยนตร์และซีรีส์ถูกปล่อยออกมา
 - age_certification : เรทของภาพยนตร์และซีรีส์ว่าเหมาะกับผู้ชมช่วงวัยไหน
 - runtime : ความยาวของภาพยนตร์และซีรส์
 - genres : ประเภทของภาพยนตร์และซีรีส์แต่ละเรื่อง
 - production_countries : ประเภทที่ผลิต
 - seasons : จำนวนซีซันของซีรีย์
 - imdb_id : id ของคะแนน imdb
 - imdb_score : คะแนน imdb 
 - imdb_votes : จำนวนคนที่โหวตให้คะแนน imdb

ข้อมูลไฟล์ที่ 2 คือ raw_credits.csv มีรายละเอียด ดังนี้
- person_id : id ของผู้เกี่ยวข้องแต่ละคน
- id : id ของภาพยนตร์และซีรีส์แต่ละเรื่อง
- name : ชื่อคนที่มีบทบาทเกี่ยวข้อง
- character : ตัวละครที่เล่นในเรื่อง
- role : บทบาทหน้าที่ในเรื่อง

# 2.Import Library and Data
## Import Library

	import sys
	import pandas as pd
	import numpy as np
	import matplotlib.pyplot as plt
	import seaborn as sns
	print( f"Python {sys.version}\nPandas {pd.__version__}\nNumPy {np.__version__}" )
	
## Import Data และแสดงรายละเอียดของข้อมูล

Import Data และแสดงรายละเอียดของข้อมูล

	df_titles = pd.read_csv('raw_titles.csv')
	df_credits = pd.read_csv('raw_credits.csv')
	
df_titles

![addpic](https://user-images.githubusercontent.com/125808327/225282746-c5f56bfa-89a4-472f-95cc-ea85e57838b2.png)

df_credits

![addpic](https://user-images.githubusercontent.com/125808327/225283709-69563361-ec81-435f-9f90-9100f8c01773.png)

# 3. Clean Data
ตรวจสอบ NaN ในแต่ละคอลัมน์
	
	df_titles.isna().sum()
จะพบ NaN ในคอลัมน์ age_certification, seasons, imdb_id, imdb_score และ imdb_votes

Drop NaN ในคอลัมน์ imdb_score, imdb_id และ imdb_votes ในตาราง df_titles

	df_titles=df_titles.dropna(subset=['imdb_score','imdb_id','imdb_votes'])
	df_titles.head()

ตรวจสอบอีกครั้งว่าคอลัมน์ที่เรานำมาวิเคราะห์ว่ามี NaN เหลืออยู่ไหมในตาราง 	

	df_titles
	df_titles.isna().sum()

ใน imdb_score, imdb_id และ imdb_votes ไม่มีแถวไหนที่เป็น NaN แล้ว จะเหลือใน age_certification และ seasons ใน seasons ที่มี NaN เพราะว่าแถวไหนที่เป็นภาพยนตร์จะไม่มีบอกว่ามีกี่ season 

ใน df_titles จะรวมข้อมูลทั้งหนังและซีรส์ จะแบ่งข้อมูลออกเป็น 2 ส่วนคือ df_show และ df_movie

df_show
แบ่งข้อมูลออกมาเฉพาะที่เป็นซีรีส์
	
	df_show = df_titles[df_titles['type']=='SHOW']
	df_show.head()
	
df_movie
แบ่งข้อมูลออกมาเฉพาะที่เป็นภาพยนตร์
	
	df_movie = df_titles[df_titles['type']=='MOVIE']
	df_movie.head()

ตรวจสอบคอลัมน์ที่เรานำมาวิเคราะห์ว่ามีค่า NaN ไหมในตาราง 

	df_show
	df_show.isna().sum()

# 4. Prepare Data and Analytic
เลือกข้อมูลมาแค่แถว ที่ imdb_score มากกว่าเท่ากับ 8 โดยตั้งชื่อว่า df_show_8
	
	df_show_8=df_show[df_show['imdb_score']>=8]
	df_show_8.head()

หาค่าเฉลี่ยของคะแนนโหวตในตาราง df_show_8

	mean_vote_8 = df_show_8['imdb_votes'].mean()
	mean_vote_8
เลือกแถวที่มีคะแนนโหวตน้อยกว่าค่าเฉลี่ยของโหวตในตาราง df_show_8 และตั้งชื่อตารางใหม่ว่า df_votes_8_lessmean

	df_votes_8_lessmean=df_show_8[df_show_8['imdb_votes']<mean_vote_8]
	df_votes_8_lessmean.head()
เรียงลำดับ imdb_score จากมากไปน้อยในตาราง df_votes_8_lessmean
	
	df_votes_8_lessmean.sort_values(by='imdb_score',ascending=False)

เรียงลำดับ imdb_votes จากมากไปน้อยในตาราง df_votes_8_lessmean

	df_votes_8_lessmean.sort_values(by='imdb_votes',ascending=False)
เลือกแถวที่มีคะแนนโหวตมากกว่าหรือเท่ากับค่าเฉลี่ยของโหวตในตาราง df_show_8 และตั้งชื่อตารางใหม่ว่า 
df_votes_8_moremean

	df_votes_8_moremean=df_show_8[df_show_8['imdb_votes']>=mean_vote_8]
	df_votes_8_moremean

เรียงลำดับ imdb_score จากมากไปน้อยในตาราง 	df_votes_8_moremean

	df_votes_8_moremean.sort_values(by='imdb_score',ascending=False)
เรียงลำดับ imdb_votes จากมากไปน้อยในตาราง df_votes_8_moremean 

	df_votes_8_moremean.sort_values(by='imdb_votes',ascending=False)
	
นับจำนวนซีรีย์แต่ละประเภทในตาราง show_genres เพื่อตรวจสอบดูว่าซีรีย์ประเภทไหนได้รับความนิยมมากที่สุด

	df_show_8.genres = df_show_8.genres.str[1:-1].str.split(',')
	show_genres = pd.Series(sum([item for item in df_show_8.genres], [])).str.replace(' ', '').value_counts().to_frame('count')
	show_genres.reset_index()
ตรวจสอบดูว่าซีรีย์ประเภท drama เรื่องไหน ได้รับความนิยมมากที่สุด

	df_show_8=df_show[df_show['imdb_score']>=8]
	df_show_8.loc[ df_show_8['genres'].str.contains('drama', case=False)].sort_values(by='imdb_votes',ascending=False).head(1)

ตรวจสอบดูว่าซีรีย์ประเภท crime เรื่องไหน ได้รับความนิยมมากที่สุด

	df_show_8.loc[ df_show_8['genres'].str.contains('crime', case=False)].sort_values(by='imdb_votes',ascending=False).head(1)

ตรวจสอบดูว่าซีรีย์ประเภท thriller เรื่องไหน ได้รับความนิยมมากที่สุด

	df_show_8.loc[ df_show_8['genres'].str.contains('thriller', case=False)].sort_values(by='imdb_votes',ascending=False).head(1)

ตรวจสอบดูว่าซีรีย์ประเภท comedy เรื่องไหน ได้รับความนิยมมากที่สุด

	df_show_8.loc[ df_show_8['genres'].str.contains('comedy', case=False)].sort_values(by='imdb_votes',ascending=False).head(1)

ตรวจสอบดูว่าซีรีย์ประเภท scifi เรื่องไหน ได้รับความนิยมมากที่สุด

	df_show_8.loc[ df_show_8['genres'].str.contains('scifi', case=False)].sort_values(by='imdb_votes',ascending=False).head(1)

นับว่าประเทศไหนผลิตซีรีย์มากที่สุด

	num = df_show_8.groupby('production_countries')['id'].apply( lambda x: x.nunique() ).to_frame()
	num
	
ตรวจสอบว่าภาพยนตร์และซีรีส์มีความนิยมมากกว่ากัน
	
	df_show_8['imdb_votes'].sum()
	df_movie_8['imdb_votes'].sum()

## ในทำนองเดียวกันเราทำการ clean data และ Prepare Data and Analytic กับ df_movie จากการวิเคราะห์ เราจะได้ว่า
## สำหรับข้อมูลซีรีส์

- ประเทศที่ผลิตซีรีย์ออกมามีคุณภาพมากที่สุดคือ สหรัฐอเมริกา

- ประเภทซีรีส์ที่เป็นที่นิยมมากที่สุดคือ Drama

- ซีรีส์ประเภท drama ที่ได้รับความนิยมมากที่สุดคือ Breaking Bad

- ซีรีส์ประเภท crime ที่ได้รับความนิยมมากที่สุดคือ Breaking Bad

- ซีรีส์ประเภท thriller ที่ได้รับความนิยมมากที่สุดคือ Breaking Bad

- ซีรีส์ประเภท comedy ที่ได้รับความนิยมมากที่สุดคือ Better Call Saul

- ซีรีส์ประเภท scifi ที่ได้รับความนิยมมากที่สุดคือ Stranger Things
 
 ## สำหรับข้อมูลภาพยนตร์

- ประเทศที่ผลิตภาพยนตร์ออกมามีคุณภาพมากที่สุดคือ อินเดีย

- ประเภทภาพยนตร์ที่เป็นที่นิยมมากที่สุดคือ Drama

- ภาพยนตร์ประเภท drama ที่ได้รับความนิยมมากที่สุดคือ Forrest Gump

- ภาพยนตร์ประเภท crime ที่ได้รับความนิยมมากที่สุดคือ Taxi Driver

- ภาพยนตร์ประเภท music ที่ได้รับความนิยมมากที่สุดคือ Inception

- ภาพยนตร์ประเภท thriller ที่ได้รับความนิยมมากที่สุดคือ Inception

- ภาพยนตร์ประเภท comedy ที่ได้รับความนิยมมากที่สุดคือ Forrest Gump
 
 
## ความนิยมของนักแสดงหรือผู้กำกับมีผลกับความนิยมภาพยนตร์และซีรีส์หรือไม่

ดูรายละเอียดข้อมูล
	
	df_credits = pd.read_csv('raw_credits.csv')
	df_credits
	
เช็คจำนวนผู้กำกับและนักแสดง

	df_credits['role'].value_counts().to_frame('count_role')
	
รวมตารางเฉพาะซีรีส์ที่ได้คะแนนโหวตมากกว่าหรือเท่ากับ8

	df_merge_show_8 = pd.merge(df_show_8,df_credits,left_on='id',right_on='id')
	df_merge_show_8
	
นับจำนวนซีรีส์ที่แสดงของแต่ละคนทั้งผู้กำกับและนักแสดง

	people_s = df_merge_show_8.groupby('name').size().sort_values(ascending=False).to_frame('count_people')
	people_s = people_s.reset_index()
	people_s
	
เราพบว่า Ahn Nae-sang (อันแนซัง) และ Aamir Khan (อาเมียร์ ข่าน) มีบทบาทเกี่ยวกับซีรีส์และภาพยนตร์ที่ได้รับความนิยมมากที่สุดตามลำดับ แต่เมื่อเรานำรายชื่อค้นหาใน google พบว่านักแสดงส่วนใหญ่ที่มีผลงานออกมาบ่อย ๆ พวกเขาไม่ได้เป็นตัวละครหลัก ดังนั้นข้อมูลชุดนี้ไม่สามารถตัดสินใจได้ว่าความนิยมของนักแสดงหรือผู้กำกับมีผลกับความนิยมภาพยนตร์และซีรีส์หรือไม่

# 5. Visualization
## สำหรับข้อมูลซีรีส์
histogram ของคะแนน imdb_score ของซีรีส์ 
จะเห็นว่าคะแนน imdb_score ส่วนใหญ่จะอยู่ที่ 8.0 - 8.2 คะแนน และ คะแนนที่มากที่สุดจะอยู่ที่ 9.6 คะแนน

![addpic](https://user-images.githubusercontent.com/125808327/225357207-ac7c15a1-570c-4f54-8932-59aefa7194a4.png)

กราฟแท่งแสดงจำนวนโหวตมากที่สุดของซีรีส์ 13 เรื่องแรก ที่จำนวนโหวตมากกว่าค่าเฉลี่ยของโหวตทั้งหมด
ซีรีส์เรื่องที่จำนวนโหวตมากที่สุดของซีรีส์ คือ Breaking Bad

![addpic](https://user-images.githubusercontent.com/125808327/225291670-0001f92c-66a2-4185-9d63-b0dc188d5ce6.png)

กราฟแท่งแสดงจำนวนโหวตมากที่สุดของซีรีส์ 10 เรื่องแรก ที่จำนวนโหวตน้อยกว่าค่าเฉลี่ยของโหวตทั้งหมด
ซีรีส์เรื่องที่จำนวนโหวตมากที่สุดของซีรีส์ คือ Manhunt

![addpic](https://user-images.githubusercontent.com/125808327/225292048-05cf5f8b-cb69-4d33-886e-0571e56131bb.png)

กราฟวงกลมแสดงจำนวนแต่ละประเภทของซีรีส์แต่ละเรื่อง
จะเห็นว่าประเภทซีรีส์ที่เป็นที่นิยมมากที่สุดคือ Drama ซึ่งมีจำนวนทั้งหมด 224 เรื่อง

![addpic](https://user-images.githubusercontent.com/125808327/225357012-5616d438-6387-4004-b404-efbc680501eb.png)

กราฟแท่งแสดงจำนวนเรื่องของซีรีส์ที่นักแสดงหรือผู้กำกับมีบทบาทที่เกี่ยวข้อง
คนที่มีผลงานมากที่สุดคือ Ahn Nae-sang (อันแนซัง) ซึ่งมีผลงานทั้งหมด 6 เรื่องและบทบาทคือเป็นนักแสดง

![addpic](https://user-images.githubusercontent.com/125808327/225319001-ba53e086-6352-41b8-8798-314b675b9bfc.png)

## สำหรับข้อมูลภาพยนตร์
histogram ของคะแนน imdb_score ของภาพยนตร์
จะเห็นว่าคะแนน imdb_score ส่วนใหญ่จะอยู่ที่ 8.0 - 8.2 คะแนน และ คะแนนที่มากที่สุดจะอยู่ที่ 9.0 คะแนน

![addpic](https://user-images.githubusercontent.com/125808327/225359059-5f438557-bb6b-4b09-bc97-0a0cf8319d85.png)

กราฟแท่งแสดงจำนวนโหวตมากที่สุดของภาพยนตร์ 11 เรื่องแรก ที่จำนวนโหวตมากกว่าค่าเฉลี่ยของโหวตทั้งหมด
ภาพยนตร์เรื่องที่จำนวนโหวตมากที่สุดของภาพยนตร์ คือ Inception

![addpic](https://user-images.githubusercontent.com/125808327/225292837-0797baad-4cbf-4d03-873a-a56c3b2581e0.png)

กราฟแท่งแสดงจำนวนโหวตมากที่สุดขอภาพยนตร์ 10 เรื่องแรก ที่จำนวนโหวตน้อยกว่าค่าเฉลี่ยของโหวตทั้งหมด
ภาพยนตร์เรื่องที่จำนวนโหวตมากที่สุดของภาพยนตร์ คือ OMG: Oh My God!

![addpic](https://user-images.githubusercontent.com/125808327/225292948-4ac5ab27-7738-43b8-a5c0-7e3f1c038475.png)

กราฟวงกลมแสดงจำนวนแต่ละประเภทของภาพยนตร์แต่ละเรื่อง
จะเห็นว่าประเภทภาพยนตร์ที่เป็นที่นิยมมากที่สุดคือ Drama ซึ่งมีจำนวนทั้งหมด 84 เรื่อง

![addpic](https://user-images.githubusercontent.com/125808327/225357171-6ee053da-fff1-4821-abac-201cb9f9d357.png)

กราฟแท่งแสดงจำนวนเรื่องของภาพยนตร์ที่นักแสดงหรือผู้กำกับมีบทบาทที่เกี่ยวข้อง
คนที่มีผลงานมากที่สุดคือ Aamir Khan (อาเมียร์ ข่าน) ซึ่งมีผลงานทั้งหมด 9 เรื่อง และบทบาทคือเป็นนักแสดง 9 เรื่อง เป็นผู้กำกับอีก 1 เรื่อง 

![addpic](https://user-images.githubusercontent.com/125808327/225319024-12b15afc-e477-4eae-9825-3f1e31f08256.png)


## กราฟดูว่าภาพยนตร์และซีรีส์ปล่อยออกในปีไหนที่ Netflix เลือกมาเข้ามาฉายเยอะที่สุด

![addpic](https://user-images.githubusercontent.com/125808327/225325988-6a7e33a5-32f4-4e39-815d-3b3e69ce9753.png)

จะเห็นว่าในเรื่องที่ปล่อยออกมาช่วงปี 1980 และNetflix เลือกเข้ามาฉายมีจำนวนซีรีย์ที่มีคะแนน imdb_score ตั้งแต่ 8 ขึ้นไปมากกว่าจำนวนภาพยนตร์ เรื่องที่ปล่อยออกมาช่วงปี 2000 มีก็เริ่มมีจำนวนซีรีส์และภาพยนตร์ที่นำเข้ามาฉายมากขึ้น ภาพยนตร์ในปี 2019 Netflix เลือกมาเข้ามาฉาย 18 เรื่อง ซีรีส์ในปี 2018 Netflix เลือกมาเข้ามาฉาย 51 เรื่อง ซึ่งเป็นปีที่เลือกเข้ามาฉายเยอะที่สุด และจะเห็นว่าภาพยนตร์และซีรีส์ในปี 2020 - 2022 ได้นำเข้ามาน้อยลงอาจจะมาจากผลกระทบของสถานการณ์โควิด


# ตอบคำถามที่สนใจ
1.  รายการหมวดไหนน่าสนใจที่สุด

	Ans.
ประเภทของซีรีส์ที่น่าสนใจมากที่สุดคือ drama
ประเภทของภาพยนตร์ที่น่าสนใจมากที่สุดคือ drama

2. ในแต่ละหมวดภาพยนตร์และซีรีส์เรื่องไหนเป็นที่นิยมมากที่สุด

	Ans.
- ซีรีส์ประเภท drama, crime, thriller ที่ได้รับความนิยมมากที่สุดคือ Breaking Bad

- ซีรีส์ประเภท comedy ที่ได้รับความนิยมมากที่สุดคือ Better Call Saul

- ซีรีส์ประเภท scifi ที่ได้รับความนิยมมากที่สุดคือ Stranger Things

- ภาพยนตร์ประเภท drama, comedy ที่ได้รับความนิยมมากที่สุดคือ Forrest Gump

- ภาพยนตร์ประเภท crime ที่ได้รับความนิยมมากที่สุดคือ Taxi Driver

- ภาพยนตร์ประเภท music, thriller ที่ได้รับความนิยมมากที่สุดคือ Inception


3. ประเทศใดผลิตภาพยนตร์และซีรีส์ออกมาแล้วเป็นที่นิยมมากที่สุด

	Ans.
ประเทศอินเดีย ผลิตภาพยนตร์ออกมาแล้วเป็นที่นิยมมากที่สุด
ประเทศสหรัฐอเมริกา ผลิตซีรีส์ออกมาแล้วเป็นที่นิยมมากที่สุด

4. ภาพยนตร์หรือซีรีส์มีความนิยมมากกว่ากัน

	Ans.
ภาพยนตร์มีความนิยมมากกว่าซีรีส์ เพราะคะแนนรวมโหวตทั้งหมดมากกว่าซีรีส์

5. ความนิยมของนักแสดงหรือผู้กำกับมีผลกับความนิยมภาพยนตร์และซีรีส์หรือไม่

	Ans.
ไม่ เพราะนักแสดงที่ได้รับบทบ่อย ๆ ส่วนใหญ่จะไม่ได้เป็นตัวละครหลัก ที่ไม่ใช่ตัวตัดสินใจว่าเราจะดูเรื่องนั้นหรือไม่ เพราะส่วนมากการตัดสินใจของเราจะอยู่ที่บทนำอย่างพระเอกนางเอก และจากที่เราพบว่า Ahn Nae-sang (อันแนซัง) และ Aamir Khan (อาเมียร์ ข่าน) มีบทบาทเกี่ยวกับซีรีส์และภาพยนตร์ที่ได้รับความนิยมมากที่สุดตามลำดับ 

6. ภาพยนตร์และซีรีส์ปล่อยออกในปีไหนที่ Netflix เลือกมาเข้ามาฉายเยอะที่สุด

	Ans.
ซีรีส์ในปี 2019 Netflix เลือกมาเข้ามาฉาย 51 เรื่อง ซึ่งเป็นปีที่เลือกเข้ามาฉายเยอะที่สุด
ภาพยนตร์ในปี 2018 Netflix เลือกมาเข้ามาฉาย 18 เรื่อง ซึ่งเป็นปีที่เลือกเข้ามาฉายเยอะที่สุด
