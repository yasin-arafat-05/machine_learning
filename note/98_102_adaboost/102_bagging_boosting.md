<br>
<br>
<br>

# `# Bagging Vs Boosting`

<br>
<br>
<br>

`আমরা ml best accuracy পায়, হয়তো, Bagging or Boosting  ব্যবহার করে । তাই, আমাদের এই দুইটার মধ্যে পার্থক্য জানা দরকার ।`


**i) Type of Model used:** আমরা জানি, ml এ আমরা LBLV model achieve করতে চাই । But, Bais and Variance are inversly proportional । To get, LBLV model we use regularization, Bagging or Boosting . 


- In Bagging we use, LBHV model example, fully grown decision tree.
- In Boosting we use, HBLV model, decision tree(max_dept=1)


**ii) Sequential VS Parallel:** 

- Bagging parallel হয় । কারণ, ডাটাসেট ভাগ হওয়ার পর, parallelly model গুলো train হয় । 
- কিন্ত, boosting এ সেইটা sequentially হয় । আগে, একটা model train হয় এরপর আরেকটা হয় । 

**iii) Weightage of BaseLearners:**

- In Bagging, while predicting the weightage of all model is same.
- In Boosting, while predicting the weigtage of all model is not same.


