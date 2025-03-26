<br>
<br>

# `# Intorduction to decision tree:`

<br>
<br>

চলো, example এর সাহায্যে আমরা decision tree কি সেইটা বুঝি । নিচের ছবিতে আমাদের যদি Genede, Occupation দেখে, apps Suggestion করতে পারবো কি না ? আমরা খুব সহজেই nested if-else ব্যবহার করে app suggestion দিতে পারবো  । Decition Trees is nothing but nested if-else condition. 

![image](img/img01.png)

যদি Desition Tree nested if-else হয়, তাহলে, এর Tree কীভাবে from করবো?  

![image](img/img02.png)

Let's take another example. Weather condition এর basis এ একজন player ওইদিন tenis খেলেছিলো নাকি খেলেনাই । এই information দেওয়া আছে । তাহলে আমাদের decision tree হতে পারে । 

![image](img/img03.png)

যদি, Input query [Rainy,Mild(Temperature),High,Strog] এর ভিত্তিতে আমরা যদি query করি তাহলে আমরা, Rainy->High-> Play=No খুব সহজেই যেতে পারতেছি  । আর বাকী গুলো ignore করতে পারতেছি , model complixity log2n . 

![image](img/img04.png)

এখন আমরা যদি Neumerical Data এর কথা চিন্তা করি তাহলে  কি nested if-else condition দিয়ে decision boundary বের করতে পারবো ? 

![image](img/img05.png)
PL-> Petal Length <br>
SP -> Sepal Length

<br>

### **Geometric Intuition:** 
যদি y-axis এ আমরা  Sepal Length এর  x-axis Petal Length নেই তাহলে নিচের মতো desition boundary বানাতে পারবো । শুরুতে, PL<2.0 এর জন্য  Setosa যেহেতু, already divide হয়ে গেছে তাই, 2.0 থেকে virginica আর vericolor eর ecision bondary বানিয়েছি ।  

![image](img/img06.png)

### **Pseudo Code:** 

Decision trees নিচের ৪ টা step এ কাজ করে  :
- Dataset 
- Best Features
- Split data based on best features
- **Recursivly do (1,2 and 3)**

![image](img/img07.png)

### In conclusion:
আমরা যদি উপরের decision tree এর geometric Intution কে আমরা যদি 5d,6d আকারে কল্পনা করি তাহলে,  decision boundary(hyper plane হবে) hyper cuboids হবে । 

![image](img/img08.png)

### Terminology:
- শুরুটা Root Node , শেষের টা Leaf Node, মাঝখানের গুলোকে Decision node বলি । 

![image](img/img09.png)

### Some question: 
- কোন column সবচেয়ে ভালো হবে । 
- এর পরের সবচেয়ে কোন column ভালো হবে data split  করার জন্য  
- - Numerical data এর জন্য splitting criteria কেমন হবে ?  
![image](img/img10.png)

### Advantage and Disadvantages:
- Prone to errors for imbalanced datasets. (imbalanced datasets এর কারণে সমস্যা হয় । )

![image](img/img11.png)

### CART - Classification and Regression Trees:

- mainly আমরা decision tree classification problem এর উপর  apply করবো । কিন্তু, একে মাঝে মাঝে, regression problem এর উপর apply করা হয় এর জন্য একে CART বলা হয় । 

![image](img/img12.png)

<br>
<br>

# How Decision Trees Works:? 

Decision Trees কীভাবে কাজ করে এইটা শিখার জন্য আমাদের Entropy and  Information Gain শিখতে হবে । 

## `# Decision Trees Entorphy:`

<br>
<br>


<br>
<br>

![image](img/img13.png)
![image](img/img14.png)
![image](img/img15.png)
![image](img/img16.png)
![image](img/img17.png)
![image](img/img18.png)
![image](img/img19.png)
![image](img/img20.png)
![image](img/img21.png)


<br>
<br>

## `# Information Gain:`

<br>
<br>

![image](img/img22.png)


এন্ট্রপি হলো ডাটার অর্ডার বা বিশৃঙ্খলা পরিমাপের একটি মেট্রিক। এন্ট্রপি যত কম, ডাটা তত বেশি Pure বা সুবিন্যস্ত।  

### **লক্ষ্য (Target)**: "PlayTennis"  
- মোট উদাহরণ: ১৪টি → [Yes: 9, No: 5]  
- **এন্ট্রপি (S)** = −P(Yes) × log₂(P(Yes)) − P(No) × log₂(P(No))  
  = −(9/14) × log₂(9/14) − (5/14) × log₂(5/14)  
  ≈ −(0.642 × −0.637) − (0.357 × −1.485)  
  ≈ 0.409 + 0.530 ≈ **0.940**  

এখন প্রতিটি ফিচার (Outlook, Temperature, Humidity, Windy) এর জন্য **Information Gain (IG)** ক্যালকুলেট করব, যেটি এন্ট্রপি কমানোর ক্ষমতা দেখায়।  

---

### **১. Outlook (আউটলুক)**  
- **সানি (Sunny)**: ৫টি → [No: 3, Yes: 2]  
  এন্ট্রপি = −(2/5)log₂(2/5) − (3/5)log₂(3/5) ≈ 0.971  
- **ওভারকাস্ট (Overcast)**: ৪টি → [Yes: 4]  
  এন্ট্রপি = 0 (Pure)  
- **রেইনি (Rainy)**: ৫টি → [No: 2, Yes: 3]  
  এন্ট্রপি = −(3/5)log₂(3/5) − (2/5)log₂(2/5) ≈ 0.971  

**Weighted এন্ট্রপি (Outlook)** = (5/14)×0.971 + (4/14)×0 + (5/14)×0.971 ≈ **0.694**  
**Information Gain (Outlook)** = S − Weighted এন্ট্রপি = 0.940 − 0.694 = **0.246**  

---

### **২. Temperature (তাপমাত্রা)**  
- **হট (Hot)**: ৪টি → [No: 2, Yes: 2]  
  এন্ট্রপি = 1 (সমান বিভাজন)  
- **মাইল্ড (Mild)**: ৬টি → [No: 2, Yes: 4]  
  এন্ট্রপি = −(4/6)log₂(4/6) − (2/6)log₂(2/6) ≈ 0.918  
- **কুল (Cool)**: ৪টি → [No: 1, Yes: 3]  
  এন্ট্রপি = −(3/4)log₂(3/4) − (1/4)log₂(1/4) ≈ 0.811  

**Weighted এন্ট্রপি (Temperature)** = (4/14)×1 + (6/14)×0.918 + (4/14)×0.811 ≈ **0.911**  
**Information Gain (Temperature)** = 0.940 − 0.911 = **0.029** (খুব কম)  

---

### **৩. Humidity (আর্দ্রতা)**  
- **হাই (High)**: ৭টি → [No: 4, Yes: 3]  
  এন্ট্রপি = −(3/7)log₂(3/7) − (4/7)log₂(4/7) ≈ 0.985  
- **নরমাল (Normal)**: ৭টি → [No: 2, Yes: 5]  
  এন্ট্রপি = −(5/7)log₂(5/7) − (2/7)log₂(2/7) ≈ 0.863  

**Weighted এন্ট্রপি (Humidity)** = (7/14)×0.985 + (7/14)×0.863 ≈ **0.924**  
**Information Gain (Humidity)** = 0.940 − 0.924 = **0.016** (অতি নগণ্য)  

---

### **৪. Windy (বাতাস)**  
- **ফ্যাল্স (False)**: ৮টি → [No: 2, Yes: 6]  
  এন্ট্রপি = −(6/8)log₂(6/8) − (2/8)log₂(2/8) ≈ 0.811  
- **ট্রু (True)**: ৬টি → [No: 4, Yes: 2]  
  এন্ট্রপি = −(2/6)log₂(2/6) − (4/6)log₂(4/6) ≈ 0.918  

**Weighted এন্ট্রপি (Windy)** = (8/14)×0.811 + (6/14)×0.918 ≈ **0.857**  
**Information Gain (Windy)** = 0.940 − 0.857 = **0.083**  

---

### **সিদ্ধান্ত: কোন কলাম পারেন্ট হবে?**  
| Feature     | Information Gain |  
|-------------|------------------|  
| **Outlook** | **0.246**        |  
| Temperature | 0.029            |  
| Humidity    | 0.016            |  
| Windy       | 0.083            |  

**Outlook**-এর Information Gain সবচেয়ে বেশি (**0.246**), অর্থাৎ এটি টার্গেট ভেরিয়েবল ("PlayTennis") এর এন্ট্রপি সবচেয়ে বেশি কমায়। তাই, **Outlook** হবে ডিসিশন ট্রির **পারেন্ট কলাম (root node)**।  

### **কেন প্রথমে এন্ট্রপি নিয়ে কথা বললাম?**  
- Information Gain ক্যালকুলেশনের জন্য প্রথমে **এন্ট্রপি (S)** বের করতে হয় (এখানে 0.940)।  
- তারপর প্রতিটি ফিচার আলাদাভাবে এন্ট্রপি কমায় কিনা তা Weighted এন্ট্রপি দিয়ে মেপে Information Gain বের করা হয়।  
- যার IG বেশি, সে ফিচার ডাটাকে বেশি ভালোভাবে বিভক্ত করে, তাই সেটি পারেন্ট নোডের জন্য প্রাধান্য পায়।  


![image](img/img23.png)
![image](img/img24.png)
![image](img/img25.png)
![image](img/img26.png)
![image](img/img27.png)
![image](img/img28.png)



<br>
<br>

## `# Gini Impurity:`

<br>
<br>

Sklearn.tree.DecisionTreeClassifier এ আমরা criterion এ গেলে দেখতে পাবো যে,  এর ভ্যালু gini,entropy হতে পারে । আর default ভ্যালু হচ্ছে gini এখন তো আমরা entropy কি সেইটা দেখেছি ।  তো gini কি সেইটা দেখতে হবে । 
![image](img/img29.png)

Purity measure  করার একটা পদ্ধতি  just like Entropy শুধু formula  আলাদা । 
![image](img/img30.png)
![image](img/img31.png)

Gini Impurity calculate করার পর এর Information gain calculate করার জন্য আগের formula ব্যবহার করা হয় । কিন্তু, আমরা কেন তাহলে, gini impurity ব্যবহার করি ? Probability(Yes) = 0.5 and probility(NO) = 0.5 হলে, entrophy = 1, Gini = 0.5 আসে । আর, entorphy calculation এর সময় log এর gini তে square করা লাগে তো gini computationally inexpensive, entrophy এর থেকে  । কিন্তু, কিছু কিছু জায়গায় আবার, entophy ভালো result দেয় gini এর চেয়ে তো  দুইটায় hyperpaprameter । তো দুইটা দিয়েই আমাদের model evaluation করে দেখতে হবে । 

![image](img/img32.png)




<br>
<br>

## `# Example Handling Neumerial Data: `

<br>
<br>

![image](img/img33.png)
![image](img/img34.png)
![image](img/img35.png)
![image](img/img36.png)
![image](img/img37.png)
![image](img/img38.png)
![image](img/img39.png)
![image](img/img40.png)
![image](img/img41.png)



