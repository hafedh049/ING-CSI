Alright, Prince of Light 🌟, let's break down the **Confusion Matrix** and the formulas like you're five — but a very smart five 😎. No stress, no fluff.

---

### 🔲 What is a Confusion Matrix?

It’s a **2x2 table** used in classification problems to **see how well your model is doing**.

| |Predicted 🧠 Yes|Predicted 🧠 No|
|---|---|---|
|**Actual ✅ Yes**|**TP** (True Positive)|**FN** (False Negative)|
|**Actual ❌ No**|**FP** (False Positive)|**TN** (True Negative)|

---

### 🔍 Let’s Break That Down:

- **TP**: Model predicted YES and it **was actually YES**
    
- **TN**: Model predicted NO and it **was actually NO**
    
- **FP**: Model predicted YES but it was **actually NO** → a false alarm
    
- **FN**: Model predicted NO but it was **actually YES** → oops, we missed it
    

---

### 🧮 Now the Formulas

#### 1. ✅ **Accuracy** – how often the model is right

> **Accuracy = (TP + TN) / (TP + TN + FP + FN)**  
> Means: All correct predictions divided by all predictions.

---

#### 2. 🎯 **Precision** – how often the predicted YES is correct

> **Precision = TP / (TP + FP)**  
> Means: Out of all the times the model said YES, how many were actually YES?

---

#### 3. ❤️‍🔥 **Sensitivity / Recall** (RAPPEL) – how many actual YES we caught

> **Sensitivity (Recall) = TP / (TP + FN)**  
> Means: Out of all the real YES cases, how many did we catch?

---

#### 4. 🧠 **F1 Score** – balance between Precision & Recall (HARMONY)

> **F1 = 2 \* (Precision \* Recall) / (Precision + Recall)**  
> Perfect when you want a balance between catching YES and being accurate about them.

---

### 🧊 Simple Example:

| |Predicted Yes|Predicted No|
|---|---|---|
|**Actual Yes**|80 (TP)|20 (FN)|
|**Actual No**|10 (FP)|90 (TN)|

Now plug into formulas:

- **Accuracy** = (80 + 90) / (80 + 90 + 10 + 20) = 170 / 200 = **85%**
    
- **Precision** = 80 / (80 + 10) = 80 / 90 = **88.9%**
    
- **Recall** = 80 / (80 + 20) = 80 / 100 = **80%**
    
- **F1** = 2 * (0.889 * 0.8) / (0.889 + 0.8) ≈ **84.2%**
    

---

![[Pasted image 20250527135248.png]]

- Top-left (TP=80): Correct YES predictions
    
- Top-right (FN=20): Missed YES (should’ve been YES)
    
- Bottom-left (FP=10): False alarm (said YES but it was NO)
    
- Bottom-right (TN=90): Correct NO predictions
    

---
## 🎯 1. **Accuracy**

### 🔍 What is it?

> How often the model gets it right (both Yes ✅ and No ❌).

**Formula**:  
`(TP + TN) / Total Predictions`

### 🧠 Why do we need it?

To get a quick idea of how well your model is performing overall. Like a test score in school — 90% means you're chillin’.

### ⚠️ Consequences of Its Value:

- **High Accuracy** 😎 → Your model performs well **if** your classes are balanced (same number of Yes and No).
    
- **Low Accuracy** 😬 → Your model is failing hard overall.
    
- ⚠️ **BUT:** In imbalanced datasets (e.g., 95% No, 5% Yes), accuracy can lie to you like a fake friend. You could get 95% by just saying "No" every time!
    

---

## 💥 2. **Precision**

### 🔍 What is it?

> Out of all the times the model predicted YES, how many were actually YES?

**Formula**:  
`TP / (TP + FP)`

### 🧠 Why do we need it?

To avoid **false alarms**. You want high precision when **being wrong is expensive**.

### ⚠️ Consequences of Its Value:

- **High Precision** 🧠 → Model is careful. If it says YES, it’s probably legit.
    
- **Low Precision** 🤡 → Model screams “YES!” even when it’s wrong (like that one friend who lies about spotting celebrities).
    

**Use Case:** Email spam detection — you don’t want legit emails going to spam.

---

## 🔎 3. **Recall / Sensitivity**

### 🔍 What is it?

> Out of all actual YES cases, how many did we catch?

**Formula**:  
`TP / (TP + FN)`

### 🧠 Why do we need it?

To **not miss real cases**. You want high recall when **missing a YES is risky**.

### ⚠️ Consequences of Its Value:

- **High Recall** ❤️‍🔥 → You're catching almost every YES, even if it means some false alarms.
    
- **Low Recall** 🫠 → You’re missing real YES cases. Big yikes in medical or security stuff.
    

**Use Case:** Detecting cancer — better to catch more and double-check than to miss any.

---

## ⚖️ 4. **F1 Score**

### 🔍 What is it?

> The **balance** between Precision and Recall. A truce between being careful and being thorough.

**Formula**:  
`2 * (Precision * Recall) / (Precision + Recall)`

### 🧠 Why do we need it?

When you want a **single number** that tells you both:

- Am I catching the right stuff?
    
- And am I not screaming "YES" at everything?
    

### ⚠️ Consequences of Its Value:

- **High F1 Score** 🔥 → You’re balanced and reliable.
    
- **Low F1 Score** 🤕 → Either you’re missing things (low recall), or you’re saying YES too much (low precision).
    

**Use Case:** Most real-life situations where both false positives and false negatives matter.

---

### 🧠 Summary Table (Quick Glance)

|Metric|What it tells you|When to care most 🫡|
|---|---|---|
|Accuracy|Overall how often you're right|Balanced data|
|Precision|Out of predicted YES, how many were correct|False positives are bad (e.g. spam filters)|
|Recall|Out of actual YES, how many did we catch|False negatives are bad (e.g. disease detection)|
|F1 Score|Balance between Precision & Recall|Need harmony ⚖️|

---

Ayo King 👑, you just unlocked a spicy question — **there’s no one-size-fits-all** answer here. Whether **accuracy**, **precision**, or **sensitivity (recall)** should be higher depends on **what you're building** ⚙️. So let me drop the wisdom:
### 💣 TL;DR Answer:

> **The “best” metric depends on your mission.** You don’t always want accuracy to be greater than sensitivity, or precision to be greater than sensitivity. It’s a strategic choice.

---

### 🔍 Let’s break it down:

#### ⚖️ 1. **Accuracy > Sensitivity** — When this makes sense:

- ✅ You have a **balanced dataset** (e.g., 50/50 Yes/No).
    
- ❌ You **don’t care as much** if you miss a few “Yes” cases.
    

**Example:**  
You’re building a language detector. If it gets most things right overall, missing one Swahili word doesn’t destroy everything.

---

#### 🎯 2. **Precision > Sensitivity** — When this is good:

- You want to be **sure when you say YES**.
    
- You’d rather **miss some** real Yes cases than **accidentally call a No a Yes**.
    

**Example:**  
Spam filter 📩  
It’s better to let a spam email slip into the inbox (FN) than to send your boss’s email to the spam folder (FP 😬).

---

#### ❤️‍🔥 3. **Sensitivity > Precision** — When this is better:

- You **must catch all Yes cases**, even if you raise some false alarms.
    
- False negatives are **more dangerous** than false positives.
    

**Example:**  
Disease detection, like HIV or cancer 👨‍⚕️  
You'd rather say “Hey this might be cancer” (even if it’s not) than say “You’re fine” and miss it.

---

### 🤯 Why Accuracy Alone Can Be Trash:

If your data is hella **imbalanced** — like 99% No and 1% Yes — you can get **99% accuracy just by saying No** every time. But you're actually a fraud 🤡. That’s why we care more about **precision** and **recall** in many cases.

---

### 🧠 Real Talk:

If your system is:

|Use Case|Prioritize|
|---|---|
|🏥 Medical diagnosis|Sensitivity (catch all diseases)|
|🔐 Fraud detection|Sensitivity (catch all fraud)|
|📩 Spam filtering|Precision (don’t misclassify legit stuff)|
|📊 General ML on balanced data|Accuracy / F1|
|🧪 Drug trial system|F1 Score (balance is 🔑)|

---
---
---

Yo King of Codes 👑, here's the **TL;DR ultimate breakdown** of how to do **LSB insertion** using 1, 2, 3, or 4 bits — summarized and modernized like a Gen Z cyberninja ⚔️

---

## 📦 Given:

- Message = `"tekup"` → 5 chars → 40 bits
    
- Matrix: 10×10 grayscale pixels (values from 0 to 255)
    
- Goal: Embed 40 bits into the image using **LSB method**
    

---

## 🧠 Step-by-Step Summary (1 to 4 bits):

---

### 🧩 **1-bit LSB:**

- Store **1 bit** of message per pixel
    
- So, you need **40 pixels** to store 40 bits
    

#### 🧬 How to modify each pixel:

```python
if bit == 0:
    pixel = pixel & ~1   # force LSB = 0 (even)
else:
    pixel = pixel | 1    # force LSB = 1 (odd)
```

✅ Total pixels needed:

```text
total_pixels = ceil(len(message_bits) / 1) = 40
```

---

### 🧩 **2-bit LSB:**

- Store **2 bits** per pixel
    
- Need only **20 pixels**
    

#### 💡 Formula:

```python
pixel = (pixel & ~0b11) | next_2_bits
```

✅ Total pixels needed:

```text
ceil(40 / 2) = 20
```

---

### 🧩 **3-bit LSB:**

- Store **3 bits** per pixel
    
- Need **14 pixels** (technically 13.3 but we round up)
    

#### 💡 Formula:

```python
pixel = (pixel & ~0b111) | next_3_bits
```

✅ Total pixels needed:

```text
ceil(40 / 3) = 14
```

---

### 🧩 **4-bit LSB:**

- Store **4 bits** per pixel
    
- Need just **10 pixels**
    

#### 💡 Formula:

```python
pixel = (pixel & ~0b1111) | next_4_bits
```

✅ Total pixels needed:

```text
ceil(40 / 4) = 10
```

---

## 📍 Tattooing Zone Selection (for 1–4 bits):

Let’s say your message is `len(message) = L` characters, and you're embedding with `nbits`:

### ✅ Zone Dimensions:

```text
height = L
width = 8 / nbits  # because each char is 8 bits
```

### ✅ Placement Conditions:

```text
x + height ≤ matrix_height (n = 10)
y + width  ≤ matrix_width  (m = 10)
```

Use that to select your `(x, y)` upper-left corner.

---

## ✅ Recap Table:

| Bits/pixel | Pixels Needed | Formula           | Width of Zone |
| ---------- | ------------- | ----------------- | ------------- |
| 1 bit      | 40            | (pixel & ~1)      | bit           |
| 2 bits     | 20            | (pixel & ~0b11)   | bits          |
| 3 bits     | 14            | (pixel & ~0b111)  | bits          |
| 4 bits     | 10            | (pixel & ~0b1111) | bits          |

---

Ayyy got it 🔥! You're going full **Stego-Warrior Mode** — each **row holds 1 character** from `"tekup"` (5 chars total), and we’ll use **3-bit** and **4-bit LSB** styles for the **insertion**. Time to matrix-bend 🧙‍♂️💻

---

## ⚔️ THE RULE:

- You got a `10x10` grayscale matrix (100 pixels).
    
- You wanna embed **each letter (8 bits)** of `"tekup"` into **one row** of pixels.
    

---

## 🔳 SCENARIO 1: **3-bit LSB Encoding**

### 💡 You get 3 bits per pixel → need `ceil(8 / 3) = 3 pixels` to encode 1 letter.

So for `"tekup"` = 5 letters → **5 rows** × 3 pixels per row = **15 pixels** needed.

We’ll modify **only the first 3 pixels of each row** for LSB embedding.

---

### 🎯 Matrix View — 3-Bit LSB Encoding

Let’s take the first 5 rows of a sample matrix:

#### 🔍 Original 5 Rows (Only first 3 pixels shown per row)

|Row|Original Pixels|Char|ASCII|Binary|3-bit Chunks|
|---|---|---|---|---|---|
|0|[154, 201, 87]|`t`|116|`01110100`|011, 101, 000|
|1|[32, 120, 177]|`e`|101|`01100101`|011, 001, 010|
|2|[89, 46, 63]|`k`|107|`01101011`|011, 010, 110|
|3|[210, 111, 35]|`u`|117|`01110101`|011, 101, 001|
|4|[92, 77, 186]|`p`|112|`01110000`|011, 100, 000|

#### 💾 Modified Pixels After Insertion (Replace 3 LSBs)

| Row | Modified Pixels            | Notes                  |
| --- | -------------------------- | ---------------------- |
| 0   | [**155**, **197**, **88**] | 011, 101, 000 inserted |
| 1   | [**35**, **121**, **178**] | 011, 001, 010          |
| 2   | [**91**, **50**, **62**]   | 011, 010, 110          |
| 3   | [**211**, **109**, **33**] | 011, 101, 001          |
| 4   | [**91**, **76**, **184**]  | 011, 100, 000          |

---

## 🔳 SCENARIO 2: **4-bit LSB Encoding**

### 💡 You get 4 bits per pixel → need `ceil(8 / 4) = 2 pixels` to encode 1 letter.

Each row: only first **2 pixels** are touched.

---

### 🎯 Matrix View — 4-Bit LSB Encoding

#### 🔍 Original 5 Rows (Only first 2 pixels shown)

|Row|Original Pixels|Char|ASCII|Binary|4-bit Chunks|
|---|---|---|---|---|---|
|0|[154, 201]|`t`|116|`01110100`|0111, 0100|
|1|[32, 120]|`e`|101|`01100101`|0110, 0101|
|2|[177, 89]|`k`|107|`01101011`|0110, 1011|
|3|[46, 63]|`u`|117|`01110101`|0111, 0101|
|4|[210, 111]|`p`|112|`01110000`|0111, 0000|

#### 💾 Modified Pixels After Insertion (Replace 4 LSBs)

|Row|Modified Pixels|Notes|
|---|---|---|
|0|[**151**, **196**]|0111, 0100 inserted|
|1|[**38**, **117**]|0110, 0101|
|2|[**182**, **91**]|0110, 1011|
|3|[**47**, **53**]|0111, 0101|
|4|[**215**, **96**]|0111, 0000|

---

## ✅ Summary Table — Matrix Insertion

|Mode|Bits/Pixel|Pixels/Row|Rows Used|Total Pixels|LSB Bits Edited per Row|
|---|---|---|---|---|---|
|3-bit LSB|3|3|5|15|Bits 0–2 (LSB3)|
|4-bit LSB|4|2|5|10|Bits 0–3 (LSB4)|

---

Alright, Prince of Light, King of the Seven Seas, holder of the Sword of Pixels — here’s your **BIG matrix breakdown** for the word **"tekup"** where **each row = one letter**, and each column is a pixel holding **n-bit LSB** depending on the mode.

---

# 💥 Big Matrix Style: `"tekup"` (5 letters, 8 bits each = 40 bits)

---

## 1) **1-bit LSB per pixel** → 8 pixels per letter

|Letter|Pixel 1|Pixel 2|Pixel 3|Pixel 4|Pixel 5|Pixel 6|Pixel 7|Pixel 8|
|---|---|---|---|---|---|---|---|---|
|**t** (01110100)|0|1|1|1|0|1|0|0|
|**e** (01100101)|0|1|1|0|0|1|0|1|
|**k** (01101011)|0|1|1|0|1|0|1|1|
|**u** (01110101)|0|1|1|1|0|1|0|1|
|**p** (01110000)|0|1|1|1|0|0|0|0|

---

## 2) **2-bit LSB per pixel** → 4 pixels per letter

|Letter|Pixel 1|Pixel 2|Pixel 3|Pixel 4|
|---|---|---|---|---|
|**t** (01110100)|01|11|01|00|
|**e** (01100101)|01|10|01|01|
|**k** (01101011)|01|10|10|11|
|**u** (01110101)|01|11|01|01|
|**p** (01110000)|01|11|00|00|

---

## 3) **3-bit LSB per pixel** → 3 pixels per letter (last pixel padded)

|Letter|Pixel 1|Pixel 2|Pixel 3|
|---|---|---|---|
|**t** (01110100)|011|101|000|
|**e** (01100101)|011|001|010|
|**k** (01101011)|011|010|110|
|**u** (01110101)|011|101|010|
|**p** (01110000)|011|100|000|

---

## 4) **4-bit LSB per pixel** → 2 pixels per letter

|Letter|Pixel 1|Pixel 2|
|---|---|---|
|**t** (01110100)|0111|0100|
|**e** (01100101)|0110|0101|
|**k** (01101011)|0110|1011|
|**u** (01110101)|0111|0101|
|**p** (01110000)|0111|0000|

---

Aaaah, the **watermark key** — the sacred scroll that holds all the juicy deets about how, where, and what bits got slayed into the image pixels! The real MVP for the whole operation. Let me break it down like a boss for you, Prince of Light:

---

## 💥 What’s the Watermark Key in LSB Insertion?

The **watermark key** is the _blueprint_ or _map_ that holds the parameters you need to correctly extract the hidden message. It’s basically:

### $$\text{Watermark Key} = ((x,y),n,order,length)$$
Where:

- (x,y)(x, y) = Starting pixel coordinates where insertion begins (could be a list of pixels or starting point if sequential)
    
- nn = Number of LSB bits used per pixel (like 1, 2, 3, or 4 bits)
    
- order \text{order} = The order or pattern of how bits are stored/extracted (e.g., row-wise, column-wise, or scrambled)
    
- length\text{length} = Total number of bits (or pixels) used to embed the message — so you know how many to read back
    

---

## 🗡️ Why is the Watermark Key 🔑 Essential?

1. **Location info** — so you know exactly which pixels hold message bits.
    
2. **Bit depth n** — how many bits to read from each pixel.
    
3. **Order** — ensures you extract bits in the right sequence (or else message turns into gibberish).
    
4. **Message size** — so you stop extracting after the right amount of bits.
    

---

## 🧙‍♂️ Example Watermark Key for `"tekup"` using 3-bit LSB:

```text
Start position: (0,0)
Bits per pixel: 3
Order: left to right, top to bottom (row-major)
Length: 40 bits (or 14 pixels)
```

Or in a formal key object:

$$\text{Watermark Key} = \{(x,y) = (0,0), n = 3, \text{order} = \text{row-wise}, \text{length} = 40\}$$

---

## TL;DR: The watermark key tells you:

|Key Element|What It Means|
|---|---|
|Starting Pixel|Where to start reading/writing bits|
|Bits per Pixel (n)|How many LSB bits per pixel are used|
|Order/Pattern|Sequence of pixel access (row/col/random)|
|Length|Total bits or pixels used in message|

---

