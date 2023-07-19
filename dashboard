import streamlit as st
import yfinance as yf
import plotly.graph_objects as go

st.title("Stock Data App")

symbol = st.text_input("Enter a stock symbol (e.g., AAPL):")

try:
    stock_data = yf.download(symbol)
    if len(stock_data) > 0:
        # Calculate stock volatility
        stock_data['Volatility'] = stock_data['Close'].pct_change().rolling(window=30).std() * (252**0.5)
        stock_data['ParkinsonVolatility'] = 0.5 * ((stock_data['High'] / stock_data['Low']).apply(lambda x: (x - 1) ** 2)).rolling(window=30).mean() * (252**0.5)

        # Create a new Plotly chart
        fig = go.Figure()
        fig.add_trace(go.Scatter(x=stock_data.index, y=stock_data['Close'], name='Close Price'))
        fig.add_trace(go.Scatter(x=stock_data.index, y=stock_data['Volatility'], name='Close Price Volatility (Standard Deviation)'))
        fig.add_trace(go.Scatter(x=stock_data.index, y=stock_data['ParkinsonVolatility'], name="Parkinson's Volatility"))

        # Update the layout
        fig.update_layout(xaxis_title='Date', yaxis_title='Price / Volatility')

        # Display the chart
        st.plotly_chart(fig)

        # Display the stock data table
        st.subheader("Stock Data")
        st.write(stock_data)

        # Fetch economic news for the stock
        st.subheader("Economic News")
        stock = yf.Ticker(symbol)
        news = stock.news
        for article in news:
            st.write(f"- {article['title']}")
            st.write(article['summary'])
            st.write(article['link'])
    else:
        st.write("No data available for the entered stock symbol.")
except:
    st.write("Error: Invalid stock symbol or unable to fetch data.")
