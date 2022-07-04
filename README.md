# BITCOIN_PRICE_PREDICTION
Bitcoin Price Prediction using Facebook Prophet, NeuralProphet, LSTM, ARMA

Long story short, I wanted to throw all the tools I learned towards predicting the future prices of Bitcoin.

We all know right now that the price of Bitcoin has dropped. There is panic in the market for sure. I was curious to what the different modeling tools will show for price predictions and trends.

The Data

I got the data here: https://coincodex.com/crypto/bitcoin/historical-data/

Notes: I had data only up to July 1, 2022. That was when I exported the csv from the website.

The Plan

I wanted to compare the results I would get from the different tools in my toolkit for time series analysis. So, I went and did the following:

Prepare/clean the data as needed to work with the modelsCreate ARMA model prediction of future Bitcoin pricesSee trends/prediction for price, volume, and market cap.Create LSTM model similar to my previous projects. Pretty much plug and play. Predict the next 30 days of price action.Train a Facebook Prophet model and predict price. Utilize its changepoints function to see major price changes.Train NeuralProphet model — This is new to me and I wanted to test it out to see what the differences were compared to the O.G. Facebook Prophet.Future Work/Wish List

ARMA Model — Good ol’ Auto-Regressive Moving Average

The plan here was to load the data and get a pretty graph. I like pretty graphs and colors.

I chose only the Closing price as the metric to focus on. I think Closing price everyday will be the most important. For Volume and Market Cap, I was curious about those.

Note: I downloaded the last 5 years of historical data for Bitcoin. Total is 1825 entries which represent each day’s prices.

A quick sneak peek at the pretty graph’s code

Boom! A pretty graph!

For the above graph, I plotted the original data against 7-day, 30-day, and 1-year moving averages. People out there use 200-day moving averages. This was just my choice.

The seasonal decomposition of the original data.

I used the seasonal_decompose() function from the Python StatsModel library to break the data down into trend, seasonality, and residuals. We see the crazy activity in residuals after 2021.

In order to choose my p and q values for ARMA model, I ran the data with lags to find ACF and PACF

It is important to note here that I did run the ADF (Augmented Dickey-Fuller) test and it gave me a pretty high p-value. However, I was still able to fit and run an ARMA model. At the end of the day, I managed to fit the model without it screaming at me that the data was not stationary. This is the best that I can hope for.

Initial ADF test

ADF test after differencing. P-Value went to almost zero

So while the ADF test initially showed a high p-value. I still managed to fit an ARMA model with it. So, I’m not sure if differencing was necessary, but I will probably revisit differencing and then running the model in the future.

ARMA Prediction of Bitcoin Price for next 75 days

The price forecast using ARMA was very tricky. I used p and q values of 8 to get any changes in the orange line. The model is imperfect, but so far so good.

Now, we look at Volume and Market Cap data. Again, I did the above steps. I decomposed the data, compared different moving averages to the original data, and then fitted an ARMA model and predicted the next 75 days.

VOLUME —

Here I compared the moving averages versus the original data. We see the 365-day average trended downward since the end of 2021.

Here, I managed to fit the ARMA model with the original data again. We see a trend downward in volume per the model.

MARKET CAP —

I thought this was pretty cool. The market cap spiked downward after going below the 1 year moving average.

Fitting the ARMA model for Market Cap data was a chore. I differenced the data and managed to get some stationary data that would fit the model. Not sure if this model gives anything useful.

LSTM Model — Long-Short Term Memory Neural Net

A quick peek at the LSTM code for building the model

This code is taken from: https://machinelearningmastery.com/how-to-develop-lstm-models-for-time-series-forecasting/

Shoutout to that smart man.

The graphed prediction looked a little too scrunched so I expanded it below

I narrowed down the date range to better see the price action prediction. Looks like it will be going crazy.

Facebook Prophet — the quick and dirty price predictor!

Always always always make those darn column names “ds” and “y” !!!

FBProphet’s decompositioni of the data. Trend shows downward

For giggles, I made FBProphet predict the next 365 days of price action. We see a downward trend.

Here we see the changepoints where there is significant rate of change in price.

You can read about changepoints here: https://facebook.github.io/prophet/docs/trend_changepoints.html

I chose a really high changepoint threshold, hopefully these changepoint areas are much more significant.

NeuralProphet — The Facebook Prophet she told you not to worry about

Neural Prophet was super easy to use. The same data prep was needed as with Facebook Prophet.

Similar to FB Prophet

The prediction results look smoother. Might be due to the AR-Neural Net that was added to Neural Prophet. This was predicting 1 year into the future.

The decomposition from NeuralProphet. I noticed the graphs are alot smoother than the Facebook Prophet’s version.

Overall, I had fun playing around with NeuralProphet. From my reading of its prowess, it is able to outperform Facebook Prophet with high amounts of data.

Future Work / Wish List

I want to predict the Volume and Market Cap data but I found predicting the ARMA model with differenced data a challenge. Definitely something I’ll have to sharpen on.I’ll need to work with the NeuralProphet documentation more. I want to get prettier graphs but will have to figure out how to do so.Facebook Prophet is great for a quick prediction. I want to keep using it for future data analysis.Automating these data flows with daily updated data would be an awesome future project.
