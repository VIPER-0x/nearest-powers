# nearest-powers
### توضیح کد برای فایل **README** در گیت‌هاب (به فارسی)

این کد یک **اندیکاتور** در زبان **Pine Script (ورژن 6)** برای پلتفرم **TradingView** است که **نزدیک‌ترین توان‌های ۲، ۳ و ۱۰** را برای مقدار ورودی مشخص شده محاسبه می‌کند.

---

## 🔹 توضیح بخش‌های کد:

### **۱. تعریف اندیکاتور**
```pinescript
indicator('Powers of 2 and 3 Table', overlay = true, dynamic_requests = true)
```
- این خط یک اندیکاتور جدید با نام **"Powers of 2 and 3 Table"** ایجاد می‌کند.
- `overlay = true` یعنی این اندیکاتور مستقیماً روی چارت اصلی رسم می‌شود.
- `dynamic_requests = true` به این معناست که مقدار ورودی می‌تواند به‌صورت پویا از تایم‌فریم‌های دیگر دریافت شود.

---

### **۲. تعریف پارامتر ورودی**
```pinescript
LOOK_BACK = input.int(defval = 1, title = "look back", minval=0, maxval=499)
```
- متغیر `LOOK_BACK` مقدار عددی **ورودی کاربر** را مشخص می‌کند که به‌عنوان تعداد کندل‌های گذشته برای گرفتن مقدار قیمت استفاده می‌شود.
- مقدار پیش‌فرض (`defval`) برابر **۱** است.
- مقدار این پارامتر باید بین **۰ تا ۴۹۹** باشد.

---

### **۳. توابع محاسبه توان‌های ۲**
#### **محاسبه نزدیک‌ترین توان پایین‌تر ۲**
```pinescript
f_get_lower_power_of_2(_in) =>
    math.pow(2, math.floor(math.log(_in) / math.log(2)))
```
- ابتدا **لگاریتم عدد ورودی بر مبنای ۲** محاسبه می‌شود.
- مقدار به پایین گرد شده (`floor`) تا نزدیک‌ترین توان پایین‌تر ۲ به دست آید.
- سپس **توان ۲** از این مقدار محاسبه شده و مقدار خروجی تولید می‌شود.

#### **محاسبه نزدیک‌ترین توان بالاتر ۲**
```pinescript
f_get_upper_power_of_2(_in) =>
    math.pow(2, math.ceil(math.log(_in) / math.log(2)))
```
- مشابه تابع قبلی است، با این تفاوت که مقدار به بالا گرد (`ceil`) می‌شود تا نزدیک‌ترین توان **بالاتر** ۲ پیدا شود.

---

### **۴. توابع محاسبه توان‌های ۳**
#### **محاسبه نزدیک‌ترین توان پایین‌تر ۳**
```pinescript
f_get_lower_power_of_3(_in) =>
    math.pow(3, math.floor(math.log(_in) / math.log(3)))
```
- این تابع مشابه توابع قبل است، اما لگاریتم بر مبنای **۳** محاسبه شده است.

#### **محاسبه نزدیک‌ترین توان بالاتر ۳**
```pinescript
f_get_upper_power_of_3(_in) =>
    math.pow(3, math.ceil(math.log(_in) / math.log(3)))
```
- مقدار به بالا گرد شده تا نزدیک‌ترین توان **بالاتر** ۳ محاسبه شود.

---

### **۵. توابع محاسبه توان‌های ۱۰**
#### **محاسبه نزدیک‌ترین توان پایین‌تر ۱۰**
```pinescript
f_get_lower_power_of_10(_in) =>
    math.pow(10, math.floor(math.log(_in) / math.log(10)))
```
- مشابه توابع قبلی، اما این بار لگاریتم بر مبنای **۱۰** محاسبه شده است.

#### **محاسبه نزدیک‌ترین توان بالاتر ۱۰**
```pinescript
f_get_upper_power_of_10(_in) =>
    math.pow(10, math.ceil(math.log(_in) / math.log(10)))
```
- مقدار به بالا گرد شده تا نزدیک‌ترین توان **بالاتر** ۱۰ به دست آید.

---

### **۶. دریافت مقدار ورودی از کندل مشخص‌شده**
```pinescript
input_value = request.security(syminfo.tickerid, "45", open[LOOK_BACK])
```
- مقدار **قیمت باز شدن (`open`)** کندل مربوط به `LOOK_BACK` را از تایم‌فریم ۴۵ دقیقه‌ای دریافت می‌کند.

---

### **۷. محاسبه نزدیک‌ترین توان‌های ۲، ۳ و ۱۰**
```pinescript
// Calculate powers of 2
lower_power_of_2 = f_get_lower_power_of_2(input_value)
upper_power_of_2 = f_get_upper_power_of_2(input_value)

// Calculate powers of 3
lower_power_of_3 = f_get_lower_power_of_3(input_value)
upper_power_of_3 = f_get_upper_power_of_3(input_value)

// Calculate powers of 10
lower_power_of_10 = f_get_lower_power_of_10(input_value)
upper_power_of_10 = f_get_upper_power_of_10(input_value)
```
- با استفاده از توابعی که قبلاً تعریف شد، مقدار **نزدیک‌ترین توان‌های ۲، ۳ و ۱۰** برای قیمت ورودی محاسبه می‌شود.

---

## 📌 **نتیجه عملکرد کد**
این اندیکاتور به شما **نزدیک‌ترین مقدار عددی از توان‌های ۲، ۳ و ۱۰** را بر اساس قیمت مشخص‌شده نمایش می‌دهد. از این اطلاعات می‌توان برای **سطوح مهم قیمتی و تحلیل‌های عددی** استفاده کرد.
