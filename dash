import streamlit as st
import pandas as pd
import numpy as np

st.title('Int-Flight Channels Performance')

df= pd.read_csv("intflight.csv", parse_dates= ["booking_date"],dtype= {"ticket" : str, "phone_number" : str, "refrence_no" : str, "utm_campaign" : str})

index1= df[df["discount_code_tag"].str.contains("Affiliate", case= False, na= False)].index
df.loc[index1,"channel_type"]= "affiliate"
index2= df[df["discount_code_tag"].str.contains("crm", case= False, na= False) | df["discount_code_tag"].str.contains("abandoned", case= False, na= False)].index
df.loc[index2,"channel_type"]= "crm"
index3= df[df["discount_code_tag"].str.contains("campaign", case= False, na= False)].index
df.loc[index3,"channel_type"]= "campaign"
index4= df[df["discount_code_tag"].str.contains("loyalty", case= False, na= False)].index
df.loc[index4,"channel_type"]= "loyalty"
index5= df[df["channel_type"].isna()].index
df.loc[index5, "channel_type"] = "other_voucher"
utm_campaign= df[df["utm_campaign"].notna()]
index6= utm_campaign[utm_campaign["utm_campaign"].str.isdigit()].index
df.loc[index6, "channel_type"] = "adwords"
index7= df[(df["discount_name"].isna()) | (~df[df["utm_campaign"].notna()]["utm_campaign"].str.isdigit())].index
df.loc[index7, "channel_type"]= "direct & organic"

df_plot= df.pivot_table(index= "issue_date", columns= "channel_type", values= "ticket", aggfunc= "count").drop(columns= ["other_voucher"])
if st.checkbox('Show raw data'):
    st.subheader('Raw data')
    st.write(df_plot)

with st.container():
    for channel_type in df_plot.columns:
        st.line_chart(df_plot[channel_type])
