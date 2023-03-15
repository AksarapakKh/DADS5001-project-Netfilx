# Topic: ความนิยมของภาพยนตร์และซีรีส์ใน Netfilx

# คำถามที่สนใจ
1.  รายการหมวดไหนน่าสนใจที่สุด

	Ans.
ประเภทของซีรีส์ที่น่าสนใจมากที่สุดคือ drama
ประเภทของซีรีส์ที่น่าสนใจมากที่สุดคือ drama

2. ประเทศใดผลิตภาพยนตร์และซีรีส์ออกมาแล้วเป็นที่นิยมมากที่สุด

	Ans.
ประเทศอินเดีย ผลิตภาพยนตร์ออกมาแล้วเป็นที่นิยมมากที่สุด
ประเทศสหรัฐอเมริกา ผลิตภาพยนตร์ออกมาแล้วเป็นที่นิยมมากที่สุด

3. ภาพยนตร์หรือซีรีส์มีความนิยมมากกว่ากัน

	Ans.
ภาพยนตร์มีความนิยมมากกว่าซีรีส์ เพราะคะแนนรวมโหวตทั้งหมดมากกว่าซีรีส์

4. ความนิยมของนักแสดงหรือผู้กำกับมีผลกับความนิยมหรือไม่

	Ans.
ไม่ เพราะว่า ไม่เหตุผลเพียงพอให้สรุป เพราะนักอสดงที่ได้รับบทบ่อย ๆ ส่วนใหญ่จะเป็นตัวประกอบที่ไม่ใช่ตัวตัดสินใจว่าเราจะดูเรื่องนั้นหรือไม่ เพราะส่วนมากการตัดสินใจของเราจะอยู่ที่บทนำ พระเอกนางเอก

# 1. Data
ข้อมูลที่ใช้ได้มาจาก [https://www.kaggle.com/datasets/thedevastator/the-ultimate-netflix-tv-shows-and-movies-dataset?select=raw_titles.csv](https://www.kaggle.com/datasets/thedevastator/the-ultimate-netflix-tv-shows-and-movies-dataset?select=raw_titles.csv)


ข้อมูลไฟล์ที่ 1 คือ raw_titles.csv มีรายละเอียด ดังนี้
 - id : id ของหนังและซีรีส์แต่ละเรื่อง
 - title : ชื่อเรื่อง
 - type : ประเภทมี 2 ประเภท คือ หนัง และ ซีรีส์
 - release_year : ปีที่หนังและซีรีส์ถูกปล่อยออกมา
 - age_certification : เรทของหนังและซีรีส์ว่าเหมาะกับผู้ชมช่วงวัยไหน
 - runtime : ความยาวของหนังและซีรส์
 - genres : ประเภทของหนังและซีรีส์แต่ละเรื่อง
 - production_countries : ประเภทที่ผลิต
 - seasons : จำนวนซีซันของซีรีย์
 - imdb_id : id ของคะแนน imdb
 - imdb_score : คะแนน imdb 
 - imdb_votes : จำนวนคนที่โหวตให้คะแนน imdb

ข้อมูลไฟล์ที่ 2 คือ raw_credits.csv มีรายละเอียด ดังนี้
- person_id : id ของผู้เกี่ยวข้องแต่ละคน
- id : id ของหนังและซีรีส์แต่ละเรื่อง
- name : ชื่อคนที่เกี่ยวข้อง
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
	
	df_show = df_titles[df_titles['type']=='SHOW']
	df_show.head()

ตรวจสอบคอลัมน์ที่เรานำมาวิเคราะห์ว่ามีค่า Nan ไหมในตาราง 

	df_show
	df_show.isna().sum()

# 4. Prepare Data and Analytic
เลือกข้อมูลมาแค่แถว ที่ imdb_score มากกว่าเท่ากับ 8 โดยตั้งชื่อว่า df_show_8
	
	df_show_8=df_show[df_show['imdb_score']>=8]
	df_show_8.head()

เรียงลำดับ imdb_votes จากน้อยไปมากในตาราง df_show_8

	df_show_8=df_show_8.sort_values(by='imdb_votes',ascending=True)
	df_show_8

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
	
นับประเภทของซีรีส์

	df_show_8.genres = df_show_8.genres.str[1:-1].str.split(',')
	show_genres = pd.Series(sum([item for item in df_show_8.genres], [])).str.replace(' ', '').value_counts().to_frame('count')
	show_genres.reset_index()
# 5. Visualization
## from df_show_8
กราฟ จำนวนโหวตมากที่สุดของซีรีส์ 13 เรื่องแรก ที่จำนวนโหวตมากกว่าค่าเฉลี่ยของโหวตทั้งหมด
ซีรีส์เรื่องที่จำนวนโหวตมากที่สุดของซีรีส์ คือ Breaking Bad
![addpic](https://user-images.githubusercontent.com/125808327/225291670-0001f92c-66a2-4185-9d63-b0dc188d5ce6.png)

กราฟ จำนวนโหวตมากที่สุดของซีรีส์ 10 เรื่องแรก ที่จำนวนโหวตน้อยกว่าค่าเฉลี่ยของโหวตทั้งหมด
ซีรีส์เรื่องที่จำนวนโหวตมากที่สุดของซีรีส์ คือ Manhunt

![addpic](https://user-images.githubusercontent.com/125808327/225292048-05cf5f8b-cb69-4d33-886e-0571e56131bb.png)


## from df_movie_8
กราฟ จำนวนโหวตมากที่สุดของภาพยนตร์ 11 เรื่องแรก ที่จำนวนโหวตมากกว่าค่าเฉลี่ยของโหวตทั้งหมด
ภาพยนตร์เรื่องที่จำนวนโหวตมากที่สุดของภาพยนตร์ คือ Inception

![addpic](https://user-images.githubusercontent.com/125808327/225292837-0797baad-4cbf-4d03-873a-a56c3b2581e0.png)

กราฟ จำนวนโหวตมากที่สุดขอภาพยนตร์ 10 เรื่องแรก ที่จำนวนโหวตน้อยกว่าค่าเฉลี่ยของโหวตทั้งหมด
ภาพยนตร์เรื่องที่จำนวนโหวตมากที่สุดของภาพยนตร์ คือ OMG: Oh My God!

![addpic](https://user-images.githubusercontent.com/125808327/225292948-4ac5ab27-7738-43b8-a5c0-7e3f1c038475.png)
