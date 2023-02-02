# MLflow Porjet - (Home Credit Risk Classification + MLflow + SHAP)
## Home Credit Risk Classification
- Pre-Operation
>
- LogisticRegression
>
- Predictions
>
- Propose a solution to Schedule  your ML pipeline
>
## MLflow
- Integration
>
- Reusable and Reproducible

![Reproduce](https://github.com/JingtaoQ/App_of_BigData/blob/main/pic/Reproduce.png)

- Make Predictions

```
Predict on a Spark DataFrame:

import mlflow
from pyspark.sql.functions import struct, col
logged_model = 'runs:/6411938243d5456280c3a47a88c342ba/model'

# Load model as a Spark UDF. Override result_type if the model does not return double values.
loaded_model = mlflow.pyfunc.spark_udf(spark, model_uri=logged_model, result_type='double')

# Predict on a Spark DataFrame.
df.withColumn('predictions', loaded_model(struct(*map(col, df.columns))))
```

```
Predict on a Pandas DataFrame:
import mlflow
logged_model = 'runs:/6411938243d5456280c3a47a88c342ba/model'

# Load model as a PyFuncModel.
loaded_model = mlflow.pyfunc.load_model(logged_model)

# Predict on a Pandas DataFrame.
import pandas as pd
loaded_model.predict(pd.DataFrame(data))
```
- MLflow UI
> ![UI](https://github.com/JingtaoQ/App_of_BigData/blob/main/pic/UI.png "UI")

## SHAP
- Build a TreeExplainer and compute Shaplay Values
    <html>
      
        explainer = shap.Explainer(log_reg, train, feature_names=features)
        shap_values = explainer(test)
      
    </html>
- Visualize explanations for a specific point of your data set

    <html>
  
        shap.plots.beeswarm(shap_values)

    </html>   

  ![Visualize1](https://github.com/JingtaoQ/App_of_BigData/blob/main/pic/p3-1.png "Visualize1")

>A red color indicates a greater impact on the credibility of the repayment, while a blue color indicates a smaller impact.

- Visualize explanations for all points of  your data set at once
    <html>

        ind = 0
        shap.plots.force(shap_values[ind],matplotlib=True)

    </html>
  
  ![Visualize2](https://github.com/JingtaoQ/App_of_BigData/blob/main/pic/P3-2.png "Visualize2")
 >The red ones are positive contributions and the blue ones are negative contributions. For the first sample, the model prediction value of 0.082263508 can be explained by the above figure as the maximum positive contribution of feature EXIT_SOURCE_3, but the negative contribution of EXIT_SOURCE_2 is large, and with the combined influence of all features, the possibility of on-time repayment for this sample individual is only 0.082.  
- Visualize a summary plot for each class on the whole dataset
    <html>

        shap.summary_plot(train, test, plot_type="bar",feature_names=features)

    </html>

  ![Visualize3](https://github.com/JingtaoQ/App_of_BigData/blob/main/pic/P3-3.png "Visualize3")
 >The influence of the conditions on the payment reputation is sorted from largest to smallest, and it can be seen from the picture that Flag mobile has the largest influence degree.
