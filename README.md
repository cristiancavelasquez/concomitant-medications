# Concomitant Medications
The main purpose of this project is to elaborate a Python script and a Power Bi dashboard in order to identify the most prescribed concomitant products in a pharmacy chain.

## Context 

The company where this project was developed is a data analysis firm specializing in the pharmaceutical sector. The suppliers of that firm (pharmacy chains) play a crucial role in the development of its economic activities. It is for this reason that significant daily efforts are made with the purpose of retaining these suppliers.

A few weeks ago, one of the largest suppliers of the company decided not to continue their partnership because the competition was offering them more favorable terms. This raised concerns among the company's executives, and I was assigned the responsibility of creating a project that could be presented to the supplier with the aim of retaining their business with us. 
>[!IMPORTANT]
> As context, this supplier holds the largest market share in my country (Colombia) and also has a presence in other Latin American countries.

Having prior knowledge of the information available to the company, I decided to create a dashboard that would enable the supplier to identify the best-selling products and their concomitant products. A concomitant product is one or more products that are typically prescribed together within a medical prescription. For example, if you visit a doctor for an infection, you will be prescribed a medication for the infection and one or two more to address the side effects that the initial product may cause.

This concomitance report is of significant importance to the supplier, as it allows them to determine, based on their best-selling products, which items they should keep in stock to avoid missing out on sales of concomitant products.

## Development

Currently, we have access to the information from medical prescriptions that are delivered to our supplier's pharmacies. In our database, we have columns containing data such as prescription ID, prescribed product, quantity, city, physician, and more. Each row represents a unique product, so there can be multiple rows associated with the same prescription. However, at this moment, we are only interested in two columns: prescription ID and the prescribed medication.

![ssh1](https://github.com/cristiancavelasquez/concomitant-medications/assets/102259605/7535347e-2c52-4827-998f-248d1f85fa16)

As inspiration for this project, I drew from the recommendation system used by Amazon, which, when you're purchasing a product, suggests items that have been frequently bought together with our primary product.

First, I defined a function called "find_pairs" which, when given a pandas Series or DataFrame 'x', generates all possible pairs of elements from 'x' and returns them as a new DataFrame with two columns "A" and "B". 

```python
def find_pairs(x):
    pairs = pd.DataFrame(list(permutations(x.values,2)),columns=["A","B"])
    return pairs
```
Secondly, I loaded the data and filtered only the necessary columns using pandas.

```python
dataset=pd.read_excel("/content/drive/MyDrive/concomitancia/concomitancia cruz verde2.xlsm")
dataset = dataset.loc[:,["Receta","descripcion"]]
```

Thirdly, I grouped the data by prescription ID, and for each group, I applied the function we previously created.

```python
dataset_combo =dataset.groupby('Receta')['descripcion'].apply(find_pairs).reset_index(drop=True)
```

After that, I calculated how often each item item_a occurs with the items in item_b.

```python
dataset_combo2 =dataset_combo.groupby(['A','B']).size()
dataset_combo2
```

Finally, I created a sorted dataframe by the most frequent combinations and exported it.

```python
dataset =dataset_combo2.reset_index()
dataset.columns = ['A','B',"Size"]
dataset.sort_values(by='Size',ascending =False, inplace =True)
dataset.head()
dataset.to_excel("result.xlsx")
```
At this point, we already have the following result: a table with 2 columns of products and the number of times they have appeared together in the same prescription: 

![ssh2](https://github.com/cristiancavelasquez/concomitant-medications/assets/102259605/018bcf69-e8a6-43ff-8cfc-f527da0cf498)

However, we cannot provide just this table to the supplier. We need to create a visual product that allows the supplier to apply filters and gain more insights. For this purpose, we utilized Power BI. In this particular case, the dashboard is straightforward, requiring the establishment of relationships among a few tables, designing visualizations, and not necessitating the use of calculated measures. To explore more complex dashboards, you can refer to my other Power BI projects.

## Results

The final outcome was a Power BI dashboard that allows the client to apply various filters, gain valuable insights, and always ensure having the necessary stock to meet customer demands, consequently leading to customer retention and increased sales. From a personal and business perspective, we successfully retained that crucial supplier, luring them away from the competition, and they are currently content with our services.

## ScreenShots

**Note:** Some of the logos and general content have been removed and/or modified in order to protect and anonymize the original data.

![1](https://github.com/cristiancavelasquez/concomitant-medications/assets/102259605/91a21f70-5b98-4ceb-922b-5a7731bfcdb3)

![2](https://github.com/cristiancavelasquez/concomitant-medications/assets/102259605/a612aa2e-8c38-416a-8205-91598b602324)

![3](https://github.com/cristiancavelasquez/concomitant-medications/assets/102259605/b6468f31-6639-4cb0-a951-3e09850851bd)

![4](https://github.com/cristiancavelasquez/concomitant-medications/assets/102259605/8f03656c-327a-4e39-a679-d7958db2ad12)

![5](https://github.com/cristiancavelasquez/concomitant-medications/assets/102259605/4b8dbb7d-fccb-45e2-a015-e3f70a9575f2)











