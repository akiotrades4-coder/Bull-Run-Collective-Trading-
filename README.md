futures-trading-system/
│
├ dashboard.py
├ bot.py
├ strategy.py
├ bot_status.txt
├ trades.csv
├ requirements.txt
└ Procfile
import pandas as pd

def generate_signal(df):

    df["ema_fast"] = df["close"].ewm(span=9).mean()
    df["ema_slow"] = df["close"].ewm(span=21).mean()

    last = df.iloc[-1]

    if last["ema_fast"] > last["ema_slow"]:
        return "LONG"

    if last["ema_fast"] < last["ema_slow"]:
        return "SHORT"

    return "NONE"
    import ccxt
import pandas as pd
import time
from strategy import generate_signal

symbol = "BTC/USDT"

exchange = ccxt.binance({
    "options": {
        "defaultType": "future"
    }
})

def bot_running():

    try:
        with open("bot_status.txt","r") as f:
            return f.read().strip() == "ON"
    except:
        return False


def fetch_data():

    ohlcv = exchange.fetch_ohlcv(symbol,"1m",limit=50)

    df = pd.DataFrame(ohlcv,columns=[
        "time","open","high","low","close","volume"
    ])

    return df


def execute_trade(signal, price):

    trade = pd.DataFrame([[time.time(),signal,price]])
    trade.to_csv("trades.csv",mode="a",header=False,index=False)

    print(signal, price)


while True:

    if bot_running():

        df = fetch_data()

        signal = generate_signal(df)

        price = df["close"].iloc[-1]

        if signal != "NONE":
            execute_trade(signal, price)

    time.sleep(20)
    import streamlit as st
import pandas as pd

st.title("Futures Trading Dashboard")

col1,col2 = st.columns(2)

with col1:
    if st.button("START BOT"):
        with open("bot_status.txt","w") as f:
            f.write("ON")

with col2:
    if st.button("STOP BOT"):
        with open("bot_status.txt","w") as f:
            f.write("OFF")

try:
    with open("bot_status.txt","r") as f:
        status = f.read()
except:
    status = "OFF"

st.subheader("Bot Status")
st.write(status)

st.subheader("Trade History")

try:

    trades = pd.read_csv("trades.csv",
    names=["time","signal","price"])

    st.dataframe(trades.tail(20))

except:
    st.write("No trades yet")
    
