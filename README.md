# App.pyimport streamlit as st
import pandas as pd
from datetime import datetime

st.set_page_config(page_title="AI Prediction System v0.1", page_icon="📈")
st.title("📈 ระบบทำนายหวยลาว (AI Prediction System v0.1)")

st.markdown('''**ขั้นตอนการใช้งาน**  
1. เตรียมไฟล์ข้อมูลหวยลาวย้อนหลังในรูปแบบ CSV  
2. อัปโหลดไฟล์ด้านล่าง  
3. ระบบจะแสดง *Top 5* เลข 2 ตัว และ 3 ตัวที่ออกบ่อยที่สุด''')

uploaded_file = st.file_uploader("📤 อัปโหลดไฟล์ข้อมูลหวยลาว (.csv)", type=['csv'])

if uploaded_file is not None:
    df = pd.read_csv(uploaded_file)
    df['number'] = df['number'].astype(str).str.zfill(4)
    df['2digit'] = df['number'].str[-2:]
    df['3digit'] = df['number'].str[-3:]

    st.subheader("🎯 Top 5 เลข 2 ตัว (นับจากความถี่)")
    top2 = df['2digit'].value_counts().head(5).reset_index()
    top2.columns = ['เลข 2 ตัว', 'จำนวนครั้ง']
    st.table(top2)

    st.subheader("🎯 Top 5 เลข 3 ตัว (นับจากความถี่)")
    top3 = df['3digit'].value_counts().head(5).reset_index()
    top3.columns = ['เลข 3 ตัว', 'จำนวนครั้ง']
    st.table(top3)
else:
    st.info("กรุณาอัปโหลดไฟล์ข้อมูลก่อนใช้งาน")