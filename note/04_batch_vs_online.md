---
---
---

<br>

# Types of ml base on how to train in production:

<br>

---
---
---

- `Production: The server in which our code in going to run know as production phase. A user will hit api of my model and use it.`

- `Development: When we code in our computer know as development phase.`

<br>
<br>

# আমাদের ml মডেল server এ আলাদা আলাদা functionality নিয়ে চলে । তার উপর ভিত্তি করে ml দুই ধরনের হয় 

- `Batch learning / offline learning `

- `Online learning `


---
---
---

<br>

# Offline or Batch Learning:

<br>
---
---
---

![Alt text](image.png)

<br>

# Problem with Batch Learning:

![Alt text](image-2.png)

# Disadvantage of Batch Learning/online Learning:

- `Lots of Data: যেহেতু আমাদের কাছে যত Data আছে সবটুকু একবারেই train করতে হবে । যদি আমাদের কাছে অনেক অনেক বেশি Data থাকে তাহলে আমরা আমাদের মডেলকে আর করতে পারবো না ।  `

- `Hardware Limitation: ধরি, একটা মডেল ২৪ ঘনটা পর পর আপডেট হয় । আমাদের যেই ডিভাইস গুলোতে আমরা মডেল টি রান করছে সেইটাতে ১ সপ্তাহ পর পর  internet এর সংযোগে আসে । তাহলে ২৪ ঘনটা পর পর আপডেট করেও আমাদের কোন লাভ হবে না । `

- `Availability: ধরি, একটা মডেল ২৪ ঘনটা পর পর আপডেট হয় । এখন যদি আমাদের আমাদের  কাছে নতুন কোন ডাটা আসলে সেইটা ২৪ ঘনটা পরই আপডেট হবে । কিন্তু, আমাদের ইউজারের কাছে ডাটা গুলো এখনই পৌঁছানো দরকার । এইক্ষেত্রে আমাদের Batch Learning faill করে ।`



