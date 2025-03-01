# Airplane Price EDA & Regression

## Description

This dataset contains 12,377 rows and is designed for machine learning models to 
predict airplane prices. The data simulates real-world factors affecting airplane 
costs, making it ideal for regression tasks, exploratory data analysis, 
and model training. Below is an explanation of the dataset's structure and columns:

### Data Understanding

Dataset: [Airplane Price Prediction](https://www.kaggle.com/datasets/asinow/airplane-price-dataset)
| Sütun Adı                  | Açıklama                                                                 |
|----------------------------|-------------------------------------------------------------------------|
| **Model**                  | Uçak modeli (örn. Boeing 737, Airbus A320). Marka ve performansı temsil eder. |
| **Üretim Yılı**            | Uçağın üretildiği yıl. Yaş hesaplamasında kullanılır.                   |
| **Motor Sayısı**           | Uçaktaki motor sayısı (1: piston, 2: diğer).                           |
| **Motor Türü**             | Motor tipi (örn. Turbofan, Piston). Performans ve yakıt verimliliğini etkiler. |
| **Kapasite**               | Yolcu kapasitesi. Daha büyük uçaklar daha yüksek fiyata sahiptir.       |
| **Menzil (km)**            | Uçağın menzili (km). Daha fazla menzil, operasyonel esneklik sağlar.    |
| **Yakıt Tüketimi (L/saat)**| Ortalama yakıt tüketimi (L/saat). Turbofan motorlar daha az yakıt tüketir. |
| **Saatlik Bakım Maliyeti (Dolar)** | Ortalama saatlik bakım maliyeti (Dolar). Yüksek maliyet, talebi düşürebilir. |
| **Yaş**                    | Uçağın yaşı (2023 - Üretim Yılı). Eski uçaklar daha düşük fiyatlıdır.   |
| **Satış Bölgesi**          | Uçağın satıldığı bölge (örn. Asya, Avrupa). Bölgesel talebi etkiler.    |
| **Fiyat (Dolar)**              | Uçağın fiyatı (Dolar). Tahmin edilecek hedef değişken.                      |

## Analyze (EDA)

<img src="https://github.com/gokhanmutlu/airplane_price_XGB_regression/blob/main/images/fiyat_dagilimi.png" width="60%">

- Farklı modellerin farklı bölgelerdeki satış fiyatlarında gözle 
görülür bir fark yok. Boeing 777'nin 
Avrupadaki satışında aykırı veri noktaları içeriyor.
<br/>

<img src="https://github.com/gokhanmutlu/airplane_price_XGB_regression/blob/main/images/fiyat_yas_dagilimi.png" width="80%">

- Genel ve doğal olarak uçağın yaşı arttıkça fiyatı aynı 
şekilde düşme eğilimi gösteriyor. Fakat bu düşüş bazı modeller için daha hızlı 
olurken bazı modeller için daha hafif bir şekilde gerçekleşmiş.
<br/>

<p float="left">
    <img src="https://github.com/gokhanmutlu/airplane_price_XGB_regression/blob/main/images/yakit_fiyat.png" width="49%">
    <img src="https://github.com/gokhanmutlu/airplane_price_XGB_regression/blob/main/images/model_kapasite.png" width="49%">
</p>

- Cessna 172 modeli için yakıt tüketimi arttıkça fiyatında herhangi 
bir değişim olmadığı gözükmekte. Bu uçak modelinin daha az kapasiteli olmasından dolayı 
kaynaklanıyor olabilir.
<br/>



<img src="https://github.com/gokhanmutlu/airplane_price_XGB_regression/blob/main/images/heatmap.png" width="60%">



- ``Üretim yılı`` ve `yaş` negatif korelasyon gösteriyor
- ``Kapasite`` ``Menzil`` birbirleri arasında yüksek korelasyon gösteriyor. Multicollinearity oluşturuyor.
- `Fiyat` ile yüksek korelasyon gösteren `Kapasite` `Menzil` özelliklerinden biri kullanılacak modele göre çıkarılabilir.

<br/>


## Construct

Veri setinde kullanmak üzere 4 farklı model oluşturuldu. Her bir model için hipermetre tuning yapıldı ve cross validation'dan yararlanıldı.

Bu modeller;
1. Linear Regression
2. AdaBoost Regression
3. Gradient Regression
4. XGB Regression

   

![Regression Models Results](https://github.com/user-attachments/assets/848a5ba7-84a6-4040-8506-b84762eff849)


## Summary

<img src="https://github.com/gokhanmutlu/airplane_price_XGB_regression/blob/main/images/feature_importance.png" width="60%">

1. Kapasite, doğrudan uçağın büyüklüğü ve taşıma kapasitesi ile ilgili olduğu için fiyat üzerinde belirleyici bir etkiye sahip.
2. Üretim Yılı ise uçakların yaşını gösterdiği için, genellikle yaşlanan uçaklar daha düşük fiyatlara sahip olur. Bu yüzden üretim yılı, modeldeki en etkili faktörlerden biri olabiliyor.
3. XGBRegression fiyatları tahminlerken yaklaşık %98 bir başarı sağlıyor. Ayrıca RMSE için de diğer modellere göre daha az sapma gösteriyor.
