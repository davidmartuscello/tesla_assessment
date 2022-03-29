# Tesla Energy Data Test

Ingestion, Visualization and Anomaly Detection for power data at Tesla Energy sites.


For analysis refer to:
- 1_ExtractData.ipynb
- 2_ProcessData.ipynb
- 3_Vizualize.ipynb
- 4_DetectAnomalies.ipynb


## Assumptions

- The sites being analyzed are ideally in close locations with similar hardware.
- Negative Solar Power value indicates an anomaly


## Types of Anomalies

Anomalies Detected in this analysis:
- Sites with negative values for solar power
- Sites with no solar power data
- Automated outlier detection based on descriptive statistics

API Failure Types:
- Missing data
- Improperly Formatted Data
- Incorrect data
- No Response

Other Anomaly Examples:
- SignalOutOfRange - Range would be specified based on engineering specification and measurement uncertainty value
- AbnormalSignature - Signal behaving out of character. Would require analysis based on domain-knowledge.
- LargePowerChange - If power change exceeds expectations
- AggregatePowerDiscrepancy - Power being unnaccounted for in combined readings


## Ideas for Improvement

Potential Improvements:
- Decompose signals into coarse sections of increasing or decreasing to capture the behavior of the signal at a higher level.
- If anomalous examples could be acquired, it would be possible to train a model to recognize those anomalies specifically
- For the anomaly detection model I could have included additional statistics (kurtosis, skewness, largest change in measurement, average change in measurement, etc.)
- I could have tried other models such as One Class SVM, or I could have used a Time Series Anomaly Detection model.

Measures to improve scalability:
- Save the data in a data lake for storage, or use something like Kafka to stream it directly into a realtime pipeline.
- Create a comprehensive data pipeline that produce visualizations and detect outliers as the data was being collected. This would require a more complicated data infrastructure, likely having dedicated services for each of these tasks.


## Diagram

In order to better understand the problem I made a simple diagram of the flow of power.

![SimplifiedDiagram.png](./SimplifiedDiagram.png)

The following equations describe the relationships between the different measurements being taken.

SitePower = SitePowerImport - SitePowerExport

SolarPower = ConsumedSolarPower + ExcessSolarPower

BatteryPower = ConsumedBatteryPower + SitePowerExport - ExcessSolarPower

PowerUse = ConsumedSolarPower + ConsumedBatteryPower + SitePowerImport

With a few more measurements, we would have a closed form solution that would allow us to check for anomalous states such as more power being consumed than is being supplied. Of course this would also require modeling efficiencies, etc.


## Additional Notes

During the 24+ hour period of downloading data from the API, there were some interruptions, so there are some gaps in the data.

Error_Log.txt was included to allow monitoring of problems without them interrupting the data processing pipline.
