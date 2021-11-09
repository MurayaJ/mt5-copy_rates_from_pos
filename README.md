from datetime import datetime
import MetaTrader5 as mt5
from time import time, sleep

# display data on the MetaTrader 5 package
print("MetaTrader5 package author: ", mt5.__author__)
print("MetaTrader5 package version: ", mt5.__version__)

# import the 'pandas' module for displaying data obtained in the tabular form
import pandas as pd

pd.set_option('display.max_columns', 500)  # number of columns to be displayed
pd.set_option('display.width', 1500)  # max table width to display

# establish connection to MetaTrader 5 terminal
if not mt5.initialize():
    print("initialize() failed, error code =", mt5.last_error())
    quit()

# get 100 GBPUSD m11 bars from the current day
while True:
    sleep(30 - time() % 30)
    rates = mt5.copy_rates_from_pos("GBPUSD", mt5.TIMEFRAME_M1, 0, 1)
    rates_frame = pd.DataFrame(rates)
    rates_frame['time'] = pd.to_datetime(rates_frame['time'], unit='s')
    rates_frame.filter(items='rows')
    print(rates_frame)




# shut down connection to the MetaTrader 5 terminal
#mt5.shutdown()
# display each element of obtained data in a new line
#print("Display obtained data 'as is'")


# create DataFrame out of the obtained data
# rates_frame = pd.DataFrame(rates)
# convert time in seconds into the datetime format


# display data
# print("\nDisplay dataframe with data")
# print(rates_frame)

# rates_frame['Open'] = [y.open for y in rates]
# rates_frame['Close'] = [y.close for y in rates]
# rates_frame['High'] = [y.high for y in rates]
# rates_frame['Low'] = [y.low for y in rates]
# rates_frame['Date'] = [y.time for y in rates]

# import plotly.graph_objs as go
# from plotly.offline import download_plotlyjs, init_notebook_mode, plot, iplot
#
# trace = go.Ohlc(x=rates_frame['time'],
#                 open=rates_frame['open'],
#                 high=rates_frame['high'],
#                 low=rates_frame['low'],
#                 close=rates_frame['close'])
#
# data = [trace]
# plot(data)
