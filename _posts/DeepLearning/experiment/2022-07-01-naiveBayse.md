---
title:  "나이브 베이즈 분류기"
categories: experiment
tag: [DeepLearning, python]
toc: true
author_profile: false
sidebar:
    nav: "docs"
---


# 인하대학교 기계학습 나이브베이즈 실습 과제
- 저는 주어진 코드가 for문을 3번반복하여 중첩으로 돌아 코드를 돌리는데 시간이 너무 오래걸리는 관계로 데이터들을 numpy로 집어넣어 3번->2번으로 최적화를 시켜 좀 더 빠른 시간내에 코드를 수행할 수 있도록 수정했습니다.





### 실습 과제 : 

### 1. fashion-mnist 데이터 셋을 gaussian naive bayseian classifier를 통해 분류 수행

![image](https://user-images.githubusercontent.com/84303857/176830972-078b2200-d9f9-4f5f-ae2d-4f03e49b882d.png)

### 2. 1번의 모델을 min-max normalization을 적용
### 3. 1번의 모델에 온라인 학습을 적용  

![image](https://user-images.githubusercontent.com/84303857/176830897-b1a792a2-d51b-42e5-88f1-3976fc6af47f.png)





<hr>

## API
```python
import numpy as np
import csv
import math
import glob
import cv2
import matplotlib.pyplot as plt
```

## 데이터 load


```python
# train data와 test data를 가져온다

train_set_path=glob.glob('./Fashion-MNIST/train/*.jpg')
train_imgs=[cv2.imread(img,cv2.IMREAD_GRAYSCALE) for img in train_set_path]
train_labels=[int(fn.split('/')[2].split('\\')[1].split('-')[2].split('.')[0]) for fn in train_set_path]


test_set_path=glob.glob('./Fashion-MNIST/test/*.jpg')
test_imgs=[cv2.imread(img,cv2.IMREAD_GRAYSCALE) for img in test_set_path]
test_labels=[int(fn.split('/')[2].split('\\')[1].split('-')[2].split('.')[0]) for fn in test_set_path]
print(len(train_imgs),len(train_labels))
print(len(test_imgs),len(test_labels))
```

    60000 60000
    10000 10000



```python
print(type(train_imgs))
```

    <class 'list'>


## 데이터가 잘 load되는지 확인


```python
plt.imshow(train_imgs[0])
plt.show()
```


​    
![output_5_0](https://user-images.githubusercontent.com/84303857/176829461-fddb8298-7c06-41fe-bee0-4ee55a1eb790.png)    


## 데이터 전처리
#### 나이브베이즈 적용을 위해 28X28 픽셀을 784X1로 변경


```python
for i in range(len(test_imgs)):
    test_imgs[i]=test_imgs[i].reshape(-1)
```


```python
for i in range(len(train_imgs)):
    train_imgs[i]=train_imgs[i].reshape(-1)
```


```python
train_imgs=np.array(train_imgs)
test_imgs=np.array(test_imgs)
train_imgs=train_imgs.astype(np.float64)
test_imgs=test_imgs.astype(np.float64)
```


```python
print(type(train_imgs))
```

    <class 'numpy.ndarray'>


## 가우시안 나이브 베이즈 정의


```python
def get_GaussianNBC(train_samples,train_labels):
    mnist_cls_0_sam=[]
    mnist_cls_1_sam=[]
    mnist_cls_2_sam=[]
    mnist_cls_3_sam=[]
    mnist_cls_4_sam=[]
    mnist_cls_5_sam=[]
    mnist_cls_6_sam=[]
    mnist_cls_7_sam=[]
    mnist_cls_8_sam=[]
    mnist_cls_9_sam=[]
  
    for k in range(len(train_samples)):
        sample=train_samples[k]
        label=train_labels[k]
        
        if label==0:
            mnist_cls_0_sam.append(sample)        
        elif label==1:
            mnist_cls_1_sam.append(sample)
        elif label==2:
            mnist_cls_2_sam.append(sample)
        elif label==3:
            mnist_cls_3_sam.append(sample)
        elif label==4:
            mnist_cls_4_sam.append(sample)
        elif label==5:
            mnist_cls_5_sam.append(sample)
        elif label==6:
            mnist_cls_6_sam.append(sample)
        elif label==7:
            mnist_cls_7_sam.append(sample)
        elif label==8:
            mnist_cls_8_sam.append(sample)
        elif label==9:
            mnist_cls_9_sam.append(sample)
            
            
    samples_by_cls=[
        mnist_cls_0_sam,
        mnist_cls_1_sam,mnist_cls_2_sam,mnist_cls_3_sam,
        mnist_cls_4_sam,mnist_cls_5_sam,mnist_cls_6_sam,
        mnist_cls_7_sam,mnist_cls_8_sam,mnist_cls_9_sam
    ]
    numOf_cls=10
    means_by_cls=[]
    stdev_by_cls=[]
    
    for C in range(numOf_cls):
        means=[]
        stdevs=[]
#         print(len(samples_by_cls[C]))
        for features in zip(*samples_by_cls[C]):
            means.append(np.mean(features))
            stdevs.append(np.std(features))
#         print(means)
        means_by_cls.append(means)
        stdev_by_cls.append(stdevs)
#     print(means_by_cls)
    means_by_cls=np.asarray(means_by_cls)
    stdev_by_cls=np.asarray(stdev_by_cls)
    print(type(means_by_cls))
    return means_by_cls,stdev_by_cls

means_by_classes,stdev_by_classes=get_GaussianNBC(train_imgs,train_labels)
```

    <class 'numpy.ndarray'>



```python
print(type(stdev_by_classes[0]))
```

    <class 'numpy.ndarray'>



```python
print(means_by_classes[0][2])
print(stdev_by_classes[0][2])
print(test_imgs[0][2])
```

    0.6953333333333334
    1.589710106347136
    0.0


## 가우시안 pdf를 numpy로 정의


```python
def Guassian_PDF(x,mean,stdev):
    a=np.zeros(784)+5
    b=np.where(stdev==0.0,1.0,a)
    c=np.where(np.logical_and(stdev==0.0 ,x==mean),0.0,b)
    d=np.where(np.logical_and(c==5.0,stdev!=0.0,x!=mean),(1/(np.sqrt(2*np.pi)*stdev))*(np.exp(-(np.power(x-mean,2)/(2*np.power(stdev,2))))),c)
    return d
```


```python
print(len(means_by_classes[0]))
```

    784


## 나이브 베이즈 분류기(Navie Bayse Classifier)


```python
def predict(means,stdevs,test_samples,labels):
    pred_classes=[]
    numOf_classes=10
    numOf_features=784
    
    for i in range(len(test_samples)):
        prob_by_classes=[]
        for C in range(numOf_classes):
            prob=1
            mean=means[C]
            stdev=stdevs[C]
            x=test_samples[i]
            prob=Guassian_PDF(x,mean,stdev)
            a=(np.sum(np.log(prob)))
            #print(a)
            prob_by_classes.append(a)
        
        prob_by_classes=np.asarray(prob_by_classes)
        pred_Label=np.argmax(prob_by_classes)
#         print('class',pred_Label)
        pred_classes.append(pred_Label)
        
    return pred_classes

pred_classes=predict(means_by_classes,stdev_by_classes,test_imgs,test_labels)
```

    C:\Users\drepi\AppData\Local\Temp\ipykernel_17896\376215071.py:5: RuntimeWarning: divide by zero encountered in true_divide
      d=np.where(np.logical_and(c==5.0,stdev!=0.0,x!=mean),(1/(np.sqrt(2*np.pi)*stdev))*(np.exp(-(np.power(x-mean,2)/(2*np.power(stdev,2))))),c)
    C:\Users\drepi\AppData\Local\Temp\ipykernel_17896\376215071.py:5: RuntimeWarning: invalid value encountered in true_divide
      d=np.where(np.logical_and(c==5.0,stdev!=0.0,x!=mean),(1/(np.sqrt(2*np.pi)*stdev))*(np.exp(-(np.power(x-mean,2)/(2*np.power(stdev,2))))),c)
    C:\Users\drepi\AppData\Local\Temp\ipykernel_17896\376215071.py:5: RuntimeWarning: invalid value encountered in multiply
      d=np.where(np.logical_and(c==5.0,stdev!=0.0,x!=mean),(1/(np.sqrt(2*np.pi)*stdev))*(np.exp(-(np.power(x-mean,2)/(2*np.power(stdev,2))))),c)
    C:\Users\drepi\AppData\Local\Temp\ipykernel_17896\569840517.py:14: RuntimeWarning: divide by zero encountered in log
      a=(np.sum(np.log(prob)))



```python
# print((pred_classes))
```

## 정확도 체크


```python
def get_Acc(pred_classes_of_testset,gt_of_testset):
    accuracy=np.equal(pred_classes_of_testset,gt_of_testset)
    return list(accuracy).count(True)/len(accuracy)*100

acc=get_Acc(pred_classes,test_labels)
print('Acc: %s'%acc)
```

    Acc: 61.08



```python
# print(test_labels)
```

# 나이브 베이즈에 min-max적용


```python
train_set_path=glob.glob('./Fashion-MNIST/train/*.jpg')
train_imgs=[cv2.imread(img,cv2.IMREAD_GRAYSCALE) for img in train_set_path]
train_labels=[int(fn.split('/')[2].split('\\')[1].split('-')[2].split('.')[0]) for fn in train_set_path]

test_set_path=glob.glob('./Fashion-MNIST/test/*.jpg')
test_imgs=[cv2.imread(img,cv2.IMREAD_GRAYSCALE) for img in test_set_path]
test_labels=[int(fn.split('/')[2].split('\\')[1].split('-')[2].split('.')[0]) for fn in test_set_path]
print(len(train_imgs),len(train_labels))
print(len(test_imgs),len(test_labels))
```

    60000 60000
    10000 10000



```python
for i in range(len(train_imgs)):
    train_imgs[i]=train_imgs[i].reshape(-1)
for i in range(len(test_imgs)):
    test_imgs[i]=test_imgs[i].reshape(-1)
```


```python
# print(train_imgs[0])
```


```python
train_imgs=np.array(train_imgs)
test_imgs=np.array(test_imgs)
train_imgs=train_imgs.astype(np.float64)
test_imgs=test_imgs.astype(np.float64)
```


```python
# print((train_imgs[0]))
```

# min-max 정규화


```python
for i in range(len(train_imgs)):
    min_value = np.min(train_imgs[i])
    max_value = np.max(train_imgs[i])
    min_value=min_value.astype(np.float64)
    max_value=max_value.astype(np.float64)
    train_imgs[i]=(train_imgs[i]-min_value)/(max_value-min_value)
```


```python
for i in range(len(test_imgs)):
    min_value = np.min(test_imgs[i])
    max_value = np.max(test_imgs[i])
    min_value=min_value.astype(np.float64)
    max_value=max_value.astype(np.float64)
    test_imgs[i]=(test_imgs[i]-min_value)/(max_value-min_value)
```


```python
# print(train_imgs[0])
```


```python
# print(test_imgs[0])
```


```python
def get_GaussianNBC(train_samples,train_labels):
    mnist_cls_0_sam=[]
    mnist_cls_1_sam=[]
    mnist_cls_2_sam=[]
    mnist_cls_3_sam=[]
    mnist_cls_4_sam=[]
    mnist_cls_5_sam=[]
    mnist_cls_6_sam=[]
    mnist_cls_7_sam=[]
    mnist_cls_8_sam=[]
    mnist_cls_9_sam=[]
  
    for k in range(len(train_samples)):
        sample=train_samples[k]
        label=train_labels[k]
        
        if label==0:
            mnist_cls_0_sam.append(sample)        
        elif label==1:
            mnist_cls_1_sam.append(sample)
        elif label==2:
            mnist_cls_2_sam.append(sample)
        elif label==3:
            mnist_cls_3_sam.append(sample)
        elif label==4:
            mnist_cls_4_sam.append(sample)
        elif label==5:
            mnist_cls_5_sam.append(sample)
        elif label==6:
            mnist_cls_6_sam.append(sample)
        elif label==7:
            mnist_cls_7_sam.append(sample)
        elif label==8:
            mnist_cls_8_sam.append(sample)
        elif label==9:
            mnist_cls_9_sam.append(sample)
            
            
    samples_by_cls=[
        mnist_cls_0_sam,
        mnist_cls_1_sam,mnist_cls_2_sam,mnist_cls_3_sam,
        mnist_cls_4_sam,mnist_cls_5_sam,mnist_cls_6_sam,
        mnist_cls_7_sam,mnist_cls_8_sam,mnist_cls_9_sam
    ]
    numOf_cls=10
    means_by_cls=[]
    stdev_by_cls=[]
    
    for C in range(numOf_cls):
        means=[]
        stdevs=[]
        for features in zip(*samples_by_cls[C]):
            means.append(np.mean(features))
            stdevs.append(np.std(features))
        means_by_cls.append(means)
        stdev_by_cls.append(stdevs)
        
    means_by_cls=np.array(means_by_cls,dtype=np.float64)
    stdev_by_cls=np.array(stdev_by_cls,dtype=np.float64)
    return means_by_cls,stdev_by_cls

means_by_classes,stdev_by_classes=get_GaussianNBC(train_imgs,train_labels)
```


```python
# print(means_by_classes[0])
```


```python
def Guassian_PDF(x,mean,stdev):
    a=np.zeros(784)+5
    b=np.where(stdev==0.0,1.0,a)
    c=np.where(np.logical_and(stdev==0.0 ,x==mean),0.0,b)
    d=np.where(np.logical_and(c==5.0,stdev!=0.0,x!=mean),(1/(np.sqrt(2*np.pi)*stdev))*(np.exp(-(np.power(x-mean,2)/(2*np.power(stdev,2))))),c)
    return d
```


```python
def predict(means,stdevs,test_samples,labels):
    pred_classes=[]
    numOf_classes=10
    numOf_features=784
    
    for i in range(len(test_samples)):
        prob_by_classes=[]
        for C in range(numOf_classes):
            prob=1
            mean=means[C]
            stdev=stdevs[C]
            x=test_samples[i]
            prob=Guassian_PDF(x,mean,stdev)
            a=((np.prod(prob)))
#             print(a)
            prob_by_classes.append(a)
        
        prob_by_classes=np.asarray(prob_by_classes)
        pred_Label=np.argmax(prob_by_classes)
#         print('class',pred_Label)
        pred_classes.append(pred_Label)
        
    return pred_classes

pred_classes=predict(means_by_classes,stdev_by_classes,test_imgs,test_labels)
```

    C:\Users\drepi\AppData\Local\Temp\ipykernel_17896\376215071.py:5: RuntimeWarning: divide by zero encountered in true_divide
      d=np.where(np.logical_and(c==5.0,stdev!=0.0,x!=mean),(1/(np.sqrt(2*np.pi)*stdev))*(np.exp(-(np.power(x-mean,2)/(2*np.power(stdev,2))))),c)
    C:\Users\drepi\AppData\Local\Temp\ipykernel_17896\376215071.py:5: RuntimeWarning: invalid value encountered in true_divide
      d=np.where(np.logical_and(c==5.0,stdev!=0.0,x!=mean),(1/(np.sqrt(2*np.pi)*stdev))*(np.exp(-(np.power(x-mean,2)/(2*np.power(stdev,2))))),c)
    C:\Users\drepi\AppData\Local\Temp\ipykernel_17896\376215071.py:5: RuntimeWarning: invalid value encountered in multiply
      d=np.where(np.logical_and(c==5.0,stdev!=0.0,x!=mean),(1/(np.sqrt(2*np.pi)*stdev))*(np.exp(-(np.power(x-mean,2)/(2*np.power(stdev,2))))),c)
    C:\Users\drepi\anaconda3\envs\rhs\lib\site-packages\numpy\core\fromnumeric.py:86: RuntimeWarning: overflow encountered in reduce
      return ufunc.reduce(obj, axis, dtype, out, **passkwargs)
    C:\Users\drepi\anaconda3\envs\rhs\lib\site-packages\numpy\core\fromnumeric.py:86: RuntimeWarning: invalid value encountered in reduce
      return ufunc.reduce(obj, axis, dtype, out, **passkwargs)



```python
# print((pred_classes))
```

## 최종 정확도


```python
def get_Acc(pred_classes_of_testset,gt_of_testset):
    accuracy=np.equal(pred_classes_of_testset,gt_of_testset)
    return list(accuracy).count(True)/len(accuracy)*100

acc=get_Acc(pred_classes,test_labels)
print('Acc: %s'%acc)
```

    Acc: 55.06999999999999


# 온라인 배치 적용


```python
train_set_path=glob.glob('./Fashion-MNIST/train/*.jpg')
train_imgs=[cv2.imread(img,cv2.IMREAD_GRAYSCALE) for img in train_set_path]
train_labels=[int(fn.split('/')[2].split('\\')[1].split('-')[2].split('.')[0]) for fn in train_set_path]

test_set_path=glob.glob('./Fashion-MNIST/test/*.jpg')
test_imgs=[cv2.imread(img,cv2.IMREAD_GRAYSCALE) for img in test_set_path]
test_labels=[int(fn.split('/')[2].split('\\')[1].split('-')[2].split('.')[0]) for fn in test_set_path]
print(len(train_imgs),len(train_labels))
print(len(test_imgs),len(test_labels))
```

    60000 60000
    10000 10000



```python
for i in range(len(train_imgs)):
    train_imgs[i]=train_imgs[i].reshape(-1)
for i in range(len(test_imgs)):
    test_imgs[i]=test_imgs[i].reshape(-1)
```


```python
train_imgs=np.array(train_imgs)
test_imgs=np.array(test_imgs)
train_imgs=train_imgs.astype(np.float64)
test_imgs=test_imgs.astype(np.float64)
```


```python
def get_GaussianNBC(train_samples,train_labels):
    mnist_cls_0_sam=[]
    mnist_cls_1_sam=[]
    mnist_cls_2_sam=[]
    mnist_cls_3_sam=[]
    mnist_cls_4_sam=[]
    mnist_cls_5_sam=[]
    mnist_cls_6_sam=[]
    mnist_cls_7_sam=[]
    mnist_cls_8_sam=[]
    mnist_cls_9_sam=[]
  
    for k in range(len(train_samples)):
        sample=train_samples[k]
        label=train_labels[k]
        
        if label==0:
            mnist_cls_0_sam.append(sample)        
        elif label==1:
            mnist_cls_1_sam.append(sample)
        elif label==2:
            mnist_cls_2_sam.append(sample)
        elif label==3:
            mnist_cls_3_sam.append(sample)
        elif label==4:
            mnist_cls_4_sam.append(sample)
        elif label==5:
            mnist_cls_5_sam.append(sample)
        elif label==6:
            mnist_cls_6_sam.append(sample)
        elif label==7:
            mnist_cls_7_sam.append(sample)
        elif label==8:
            mnist_cls_8_sam.append(sample)
        elif label==9:
            mnist_cls_9_sam.append(sample)
            
            
    samples_by_cls=[
        mnist_cls_0_sam,
        mnist_cls_1_sam,mnist_cls_2_sam,mnist_cls_3_sam,
        mnist_cls_4_sam,mnist_cls_5_sam,mnist_cls_6_sam,
        mnist_cls_7_sam,mnist_cls_8_sam,mnist_cls_9_sam
    ]
    numOf_cls=10
    means_by_cls=[]
    stdev_by_cls=[]
    for C in range(numOf_cls):
        means=[]
        stdevs=[]
        mu=[]
        i=1
        stdev=0
        for features in zip(*samples_by_cls[C]):
            mu=features[0]
            sigma=0
            # 평균과 분산을 배치마다 update 시켜준다
            for i in range(1,len(features)):
                sigma=sigma+((((features[i]-mu)**2)/i)\
                             -(((features[i]-mu)**2)/i**2)-sigma/i)
                mu=mu+((features[i]-mu)/k)
#             print(features)
            means.append(mu)
            stdevs.append(sigma)
#             means.append(np.mean(features))
#             stdevs.append(np.std(features))
        means_by_cls.append(means)
        stdev_by_cls.append(stdevs)

        
    means_by_cls=np.array(means_by_cls)
    stdev_by_cls=np.array(stdev_by_cls)
    return means_by_cls,stdev_by_cls

means_by_classes,stdev_by_classes=get_GaussianNBC(train_imgs,train_labels)
```


```python
print(means_by_classes.shape)
```

    (10, 784)



```python
def predict(means,stdevs,test_samples,labels):
    pred_classes=[]
    numOf_classes=10
    numOf_features=784
    
    for i in range(len(test_samples)):
        prob_by_classes=[]
        for C in range(numOf_classes):
            prob=1
            mean=means[C]
            stdev=stdevs[C]
            x=test_samples[i]
            prob=Guassian_PDF(x,mean,stdev)
            a=(np.sum(np.log(prob)))
#             print(a)
            prob_by_classes.append(a)
        
        prob_by_classes=np.asarray(prob_by_classes)
        pred_Label=np.argmax(prob_by_classes)
#         print('class',pred_Label)
        pred_classes.append(pred_Label)
        
    return pred_classes

pred_classes=predict(means_by_classes,stdev_by_classes,test_imgs,test_labels)
```

    C:\Users\drepi\AppData\Local\Temp\ipykernel_17896\509758389.py:14: RuntimeWarning: divide by zero encountered in log
      a=(np.sum(np.log(prob)))
    C:\Users\drepi\AppData\Local\Temp\ipykernel_17896\376215071.py:5: RuntimeWarning: divide by zero encountered in true_divide
      d=np.where(np.logical_and(c==5.0,stdev!=0.0,x!=mean),(1/(np.sqrt(2*np.pi)*stdev))*(np.exp(-(np.power(x-mean,2)/(2*np.power(stdev,2))))),c)
    C:\Users\drepi\AppData\Local\Temp\ipykernel_17896\376215071.py:5: RuntimeWarning: invalid value encountered in true_divide
      d=np.where(np.logical_and(c==5.0,stdev!=0.0,x!=mean),(1/(np.sqrt(2*np.pi)*stdev))*(np.exp(-(np.power(x-mean,2)/(2*np.power(stdev,2))))),c)
    C:\Users\drepi\AppData\Local\Temp\ipykernel_17896\376215071.py:5: RuntimeWarning: invalid value encountered in multiply
      d=np.where(np.logical_and(c==5.0,stdev!=0.0,x!=mean),(1/(np.sqrt(2*np.pi)*stdev))*(np.exp(-(np.power(x-mean,2)/(2*np.power(stdev,2))))),c)



```python
# print(pred_classes)
```


```python
# print(test_labels)
```


```python
def get_Acc(pred_classes_of_testset,gt_of_testset):
    accuracy=np.equal(pred_classes_of_testset,gt_of_testset)
    return list(accuracy).count(True)/len(accuracy)*100

acc=get_Acc(pred_classes,test_labels)
print('Acc: %s'%acc)
```

    Acc: 31.480000000000004



```python

```
