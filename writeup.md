# Local Authority - picoCTF Write-up

## **Challenge Overview**

* **Category:** Web Exploitation
* **Difficulty:** Easy

---

## **Thought Process & Solution Approach**

By opening **Developer Tools** and reviewing JavaScript files, I quickly identified hardcoded credentials within the source code. Rather than authenticating on the backend, the site directly checks for specific username-password combinations in JavaScript.

Exploiting this was straightforward—I simply used the credentials found in the JavaScript:

* **Username:** `admin`
* **Password:** `strongPassword098765`

This allowed me to bypass authentication and retrieve the flag.

---

# Cookies - picoCTF Write-up

## **Challenge Overview**

* **Category:** Web Exploitation
* **Difficulty:** Easy

---

## **Thought Process & Solution Approach**

Upon inspecting the challenge page, I noticed a cookie named `name` with a value of `-1`. Modifying this value manually resulted in different responses, suggesting that the flag requires a specific value.

Observing this, I also tried to find out the max value by testing `50`, `25`, and so on (**binary search**) to determine the highest designated cookie value. I figured we run out of cookies at some point, and found that **28** was the limit.

---

## **Script Development & Execution**

Since the server responded differently for each cookie value, I created a script to iterate through a range of values. The approach:

1. Send HTTP requests using `curl` while modifying the cookie.
2. Capture responses to identify patterns.
3. Determine the value that returns the flag.

The script:

```bash
for i in {0..28}; do
    curl --cookie "name=$i" "http://mercury.picoctf.net:29649/check"
done
```

---

# Scavenger Hunt - picoCTF Write-up

## **Challenge Overview**

* **Category:** Web Exploitation
* **Difficulty:** Easy

---

## **Thought Process & Solution Approach**

As soon as I saw the name **Scavenger Hunt**, I knew this challenge would involve searching for hidden elements across the webpage. The first thing I did was inspect the **HTML source**, expecting to find clues in comments.

### **Searching for Flag Fragments**

1. **Looking at the Page Source**
   I opened **Developer Tools** and searched for comments in the HTML. As expected, I found the first piece:
   **`picoCTF{t`**

2. **Exploring the CSS File**
   Since stylesheets often contain hidden notes, I checked `mycss.css`. A comment inside revealed **part 2**:
   **`h4ts_4_l0`**

3. **Checking `robots.txt` for Restrictions**
   Seeing a hint about **search engine indexing** in the JavaScript file, I thought about `robots.txt`, which websites use to block crawlers. Sure enough, visiting `/robots.txt` revealed **part 3**:
   **`t_0f_pl4c`**

4. **Following Apache Server Clues**
   The `robots.txt` file hinted that the site might be running **Apache**, so I investigated common server configuration files like `.htaccess`. Accessing `/`.htaccess gave **part 4**:
   **`3s_2_lO0k`**

5. **Noticing the Mac Reference**
   The `.htaccess` file contained a comment about Mac file storage. Immediately, I thought of `.DS_Store`, which **macOS automatically creates for directories**. Sure enough, visiting `/.DS_Store` revealed **part 5**:
   **`_fa04427c}`**

---

## **Final Flag**

After collecting all parts, I pieced them together:
**`picoCTF{th4ts_4_l0t_0f_pl4c3s_2_lO0k_fa04427c}`**

---

# 📌 The Numbers - picoCTF Write-up

## **Challenge Overview**

* **Category:** Cryptography
* **Difficulty:** Easy

---

## **Thought Process & Solution Approach**

The challenge presents a sequence of numbers with the prompt: **"The numbers... what do they mean?"** This immediately suggests a **number-to-letter substitution cipher**, commonly known as **A1Z26**, where A=1, B=2, ..., Z=26.

To decode the message, I mapped each number to its corresponding letter.

### **Decoding the Numbers**

1. **Mapping Using A1Z26 Cipher**

   * `16 9 3 15 3 20 6` → **P I C O C T F**
   * `{ 20 8 5` → **{ T H E**
   * `14 21 13 2 5 18 19` → **N U M B E R S**
   * `13 1 19 15 14 }` → **M A S O N }**

2. **Flag Assembly**

   * Combining all parts:
     **`picoCTF{THENUMBERSMASON}`**

---

## **Final Flag**

**`picoCTF{THENUMBERSMASON}`**

---

# 📌 Obedient Cat - picoCTF Write-up

## **Challenge Overview**

* **Category:** General Skills
* **Difficulty:** Easy

---

## **Understanding the Challenge**

The challenge provides a file and hints that we should use a command-line tool to view its contents. The name **"Obedient Cat"** suggests using the `cat` command, which is used in Linux to display file contents.

---

## **Solution Approach**

* The challenge provides a file download the file and open it.

---

# 📌 Glitch Cat - picoCTF Write-up

## **Challenge Overview**

* **Category:** General Skills
* **Difficulty:** Easy

---

## **Understanding the Challenge**



---

## **Solution Approach**

### **Step 1: Connect to the Server**

* Using the provided command, I established a connection:

  ```bash
  nc saturn.picoctf.net 61848
  ```
* The output contained a string with a mix of characters and `chr()` functions.

### **Step 2: Extract the Encoded Characters**

* The flag format was clear:

  ```
  'picoCTF{gl17ch_m3_n07_' + chr(0x62) + chr(0x64) + chr(0x61) + chr(0x36) + chr(0x38) + chr(0x66) + chr(0x37) + chr(0x35) + '}'
  ```
* I identified the numeric values inside `chr()`.

### **Step 3: Decode the Hex Values**

* Using Python, I converted the `chr()` values into readable characters:

  ```python
  print('picoCTF{gl17ch_m3_n07_' + chr(0x62) + chr(0x64) + chr(0x61) + chr(0x36) + chr(0x38) + chr(0x66) + chr(0x37) + chr(0x35) + '}')
  ```
* This revealed the final flag.

---

## **Final Flag**

**`picoCTF{gl17ch_m3_n07_bda68f75}`**

---
# VaultDoorTraining

## **Challenge Overview**

* **Category:** Reverse Engineering
* **Difficulty:** Easy

---

## **Thought Process & Solution Approach**

This was my first encounter with a Java-based binary challenge. I knew immediately that I’d have to decompile the `.class` file. After doing that, I scanned the code for anything that looked like a password check.

I came across this line early:

```java
String input = userInput.substring("picoCTF{".length(), userInput.length()-1);
```

This was a big clue—it meant the actual password logic is comparing only what’s inside the `{}` of the `picoCTF{...}` format.

Then I found this hardcoded check:

```java
return password.equals("w4rm1ng_Up_w1tH_jAv4_3808d338b46");
```

So it was simply comparing the input with a fixed string. No fancy decoding or transformation. It felt more like a warm-up to get used to navigating Java decompiled logic.

Having confirmed that, I reconstructed the full flag in the correct format.

---

## **Final Flag**

**`picoCTF{w4rm1ng_Up_w1tH_jAv4_3808d338b46}`**

---
# bit-o-asm-1 - picoCTF Write-up

## **Challenge Overview**

* **Category:** Reverse Engineering
* **Difficulty:** Very Easy

---

## **Thought Process & Solution Approach**

This challenge gave me a disassembled assembly snippet and asked me to determine the value that would be stored in the `eax` register.

I carefully read the instructions and found the key instruction in the provided code:

```asm
<+15>:    mov    eax,0x30
```

This made it very straightforward—the `eax` register is being directly loaded with the hexadecimal value `0x30`.

I used a simple conversion to decimal:

```bash
0x30 = 48
```

That’s all I needed. The flag was simply wrapped around that value in standard picoCTF format.

---

## **Final Flag**

**`picoCTF{48}`**

---

# bit-o-asm-2 - picoCTF Write-up

## **Challenge Overview**

* **Category:** Reverse Engineering
* **Difficulty:** Very Easy

---

## **Thought Process & Solution Approach**

This was almost identical to *bit-o-asm-1* in structure. The difference was that instead of a direct move to `eax`, the value was first moved into memory and then copied into `eax`.

Here’s what the relevant snippet looked like:

```asm
<+15>:    mov    DWORD PTR [rbp-0x4],0x9fe1a
<+22>:    mov    eax,DWORD PTR [rbp-0x4]
```

So essentially, `eax` ends up with the value `0x9fe1a`. Again, I converted that hex to decimal:

```bash
0x9fe1a = 654874
```

That gave me the flag in the required format.

---

## **Final Flag**

**`picoCTF{654874}`**

---

# tunn3l\_v1s10n - picoCTF Write-up

## **Challenge Overview**

* **Category:** Forensics
* **Difficulty:** Medium

---

## **Thought Process & Solution Approach**

This challenge provided a file named `tunn3l_v1s10n`. I started by identifying its type:

```bash
file tunn3l_v1s10n
```

It showed up as just "data", so I knew I had to inspect it manually. I ran a hex viewer on it and immediately noticed the `BM` header, which is common for BMP files. That told me the file was meant to be a bitmap image, but something was clearly off with the header values.

---

## **Fixing the Header**

To visualize the image correctly, I had to patch a few things:

1. **Data Offset (bytes 10–13):**
   Was corrupted—replaced it with `36 00 00 00` (which is 54 in decimal, the standard BMP pixel array offset).

2. **DIB Header Size (bytes 14–17):**
   Should be `28 00 00 00` (40 bytes), but was incorrect in the dump.

3. **Height Value (bytes 22–25):**
   I increased the height from `0x132` (306) to `0x332` (818) to force it to reveal more vertical content hidden beyond the cropped height.

I used a hex editor (like `hexedit` or `bless`) to make these patches and saved the file. After reopening the image using a viewer, I could now see hidden content at the bottom which revealed the flag.

---

## **Final Flag**

**`picoCTF{qu1t3_a_v13w_2020}`**

---
