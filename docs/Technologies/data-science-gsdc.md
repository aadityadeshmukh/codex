# Data Science

Notes from the "Certified Data Science Professional" GSDC certification training 

Statistics
??? info
    - Statistics - Science of numerical data collected, analysed, interpret and present data
    - Types:
        - Descriptive Stats:
            - Used to describe data
            - Types:
                - Measure of central tendency: 
                    - mean : Average
                    - median : Central value
                    - mode : Most often value
                - Measure of spread: range, standard deviation
        - Prescriptive Stats:
            - Used to make decisions or predictions
            - Uses probability to determine confidence
    - Dispersion using quartiles:
        - Percentile means the data set is divided in 100 parts
        - Quartile means the data set is divided in 4 parts
        - Outliers:
            - An observation is unusual if:
                - it is less than 1.5 times IQR below first quartile
                - it is more than 1.5 times IQR 3rd quartiles
    - Types of Data:
        | Type | Examples |
        | ---- | -------- |
        | Structured | RDBMS, CSV |
        | Semi-structured | XML, JSON |
        | Unstructured | Images, video, IOT, health |
    - Univariate analysis
        - Single variable
    - Bivariate
        - Dependent variable is a function of independent variable
    - Multivariate
        - More than one or 2 variables used in analysis
    - Code samples:
        ```py
            import numpy as np
            import matplotlib.pyplot as plt
            import seaborn as sns
            import statistics as stats
            import pandas as pd

            # Create array
            a = np.array([1,4,5,4,8])

            # Finding mean using numpy module
            a.mean()
            ages = np.random.randint(18, 90, 800)
            # Finding mode \ median using stats module
            stats.mode(ages)
            stats.median(ages)

            # creating a set of random data with normal value
            incomes = np.random.normal(27000, 15000, 10000)
            np.mean(incomes)

            # plotting a histogram
            plt.hist(incomes, 100, ec='white')

            # Finding variance
            variance = a1.var()

            # finding standard deviation
            std = a1.std()

            # Box plot, percentile, quartiles, inter quartile range

            a1 = np.array([85, 100, 95, 92, 99, 103, 97, 110, 99, 102, 120, 90, 94])
            sns.boxplot(a1)

            q1 = np.percentile(a1,25)
            q3 = np.percentile(a1,75)

            iqr = q3-q1

            lower = q1 - (1.5*iqr)
            upper = q3 + (1.5*iqr)

            # saving figure in a location
            plt.plot(a1)
            plt.saveFig('location on disk')

            # reading data from a file
            fileData = pd.read_csv('location')
            file.head() # gives data from a file

            # plotting data from columns
            plt.plot(fileData['columnName'], fileData['columnName'])

            # counting frequency of a value in a column
            fileData.<ColumnName>.value_counts()

            # creating a heatmap using seaborn
            # This specific case is used to check null value
            sns.heatmap(fileData.isnull())

            # finding co-relation between various fields
            sns.heatmap(fileData.corr(),cmap="coolwarm")

            # Pie plots
            cities = ["Pune", "Mumbai", "Delhi"]
            engineers = [230,333,421]
            plt.pie(engineers, labels=cities, autopct='%.2f%%')

            # PLotting a value plot
            sns.valueplot(x='ColumnName', hue='ColumnName', data = fileData)
        ```
