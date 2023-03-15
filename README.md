# DADS5001-project-Netfilx

# คำถามที่สนใจ
1.  รายการหมวดไหนน่าสนใจที่สุด

	Ans.
ประเภทของซีรีส์ที่น่าสนใจมากที่สุดคือ drama
ประเภทของซีรีส์ที่น่าสนใจมากที่สุดคือ drama

2. ประเทศใดผลิตหนังและซีรีส์ออกมาแล้วเป็นที่นิยมมากที่สุด

	Ans.
ประเทศอินเดีย ผลิตหนังออกมาแล้วเป็นที่นิยมมากที่สุด
ประเทศสหรัฐอเมริกา ผลิตหนังออกมาแล้วเป็นที่นิยมมากที่สุด

3. หนังหรือซีรีส์มีความนิยมมากกว่ากัน

	Ans.
หนังมีความนิยมมากกว่าซีรีส์ เพราะคะแนนรวมโหวตทั้งหมดมากกว่าซีรีส์

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
	df_credits

![addpic](https://drive.google.com/drive/u/0/folders/16kSSiiyYg4jiXN_YMLVvL-Wnf2nBCEU3)

## Clean data
ตรวจสอบ Nan ในแต่ละคอลัมน์
	
	df_titles.isna().sum()

Drop NaN ในคอลัมน์ imdb_score, imdb_id และ imdb_votes ในตาราง df_titles

	df_titles=df_titles.dropna(subset=['imdb_score','imdb_id','imdb_votes'])
	df_titles.head()

ตรวจสอบคอลัมน์ที่เรานำมาวิเคราะห์ว่ามี Nan มั้ยในตาราง 	

	df_titles
	df_titles.isna().sum()


ใน df_titles จะรวมข้อมูลทั้งหนังและซีรส์ จะแบ่งข้อมูลออกเป็น 2 ส่วนคือ df_show และ df_movie

df_show
แบ่งข้อมูลออกมาเฉพาะที่เป็นซีรีส์
	
	df_show = df_titles[df_titles['type']=='SHOW']
	df_show.head()
	
df_movie
แบ่งข้อมูลออกมาเฉพาะที่เป็นหนัง
	
	df_show = df_titles[df_titles['type']=='SHOW']
	df_show.head()
ตรวจสอบคอลัมน์ที่เรานำมาวิเคราะห์ว่ามีค่า Nan มั้ยในตาราง 

	df_show
	df_show.isna().sum()

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
# 3.
