# SETUP.md — คู่มือติดตั้งเครื่องมือสำหรับโปรเจกต์ TaskFlow

> ไฟล์นี้คือ "แหล่งความจริงเรื่องเวอร์ชันและคำสั่งติดตั้ง" ของทั้ง bundle.
> ไฟล์อื่น (CURRICULUM, LEARNING-PLAN, CLAUDE.md ฯลฯ) จะอ้างกลับมาที่นี่ ห้าม pin เวอร์ชันที่อื่น.
> ใช้ในเฟส **Phase 0 — Setup & ปฐมนิเทศ** เป็นหลัก แต่ Phase 8 (deploy) จะกลับมาใช้ส่วน GitHub/Vercel อีกครั้ง.

---

## 0. คำสั่งสำหรับ Claude เครื่องปลายทาง (อ่านก่อนเริ่มสอน) — สำคัญมาก

ผู้เรียนเป็น **มือใหม่ที่ไม่เคยเขียนโปรแกรม** ทำตามนี้เป๊ะ:

1. **ตรวจ OS ของผู้เรียนก่อนเสมอ** อย่าเดา. ถามตรงๆ หรือให้ผู้เรียนรันคำสั่งเช็ค แล้ว **เลือกชุดคำสั่งให้ตรง OS เดียว** ไม่ต้องแปะคำสั่งของ OS อื่นให้ผู้เรียนสับสน.
   - วิธีถาม: "ตอนนี้ใช้คอมระบบอะไรอยู่ครับ — Windows, Mac, หรือ Linux?" ถ้าไม่แน่ใจ ให้ผู้เรียนเปิด terminal แล้วรันคำสั่งตรวจ OS (ดูข้อ 1).
2. **ทำทีละขั้น (hand-holding)** ขั้นละ 1 เรื่อง. สั่งให้ติดตั้ง 1 อย่าง → ให้รันคำสั่งเช็ค → ยืนยันผลลัพธ์ → ค่อยไปขั้นถัดไป. ห้ามเทคำสั่งรวด.
3. **อธิบาย "ทำไม" สั้นๆ ทุกขั้น** เช่น "ติดตั้ง Node.js เพราะมันคือตัวรัน JavaScript นอกเบราว์เซอร์ ที่ Next.js ต้องใช้" — 1-2 ประโยคพอ.
4. **คำสั่ง terminal**: ผู้เรียนต้องพิมพ์/วางคำสั่งใน terminal เป็น. บน Windows ให้ใช้ **PowerShell** (ไม่ใช่ cmd). บน Mac ใช้ **Terminal** (zsh). บอกวิธีเปิดให้ชัด (ดูข้อ 1).
5. **เช็คทุกอย่างก่อนไป Phase 1**: จบ Phase 0 ผู้เรียนต้องมี Node, Git, VS Code, บัญชี GitHub, บัญชี Vercel, และรัน `create-next-app` ขึ้นเว็บใน browser ได้ครบ.
6. ถ้าคำสั่งใดพัง → ไปดูหมวด **Troubleshooting** (ข้อ 9) ก่อนลองสุ่มแก้.

**เวอร์ชันที่ตรวจสอบ ณ มิถุนายน 2026 (ใช้เป็นค่าอ้างอิง):**

| เครื่องมือ | เวอร์ชันล่าสุด/ที่แนะนำ | หมายเหตุ |
|---|---|---|
| Node.js | **24.x LTS** (ล่าสุด v24.18.0, สาย "Active LTS" — ดูเลข/codename ล่าสุดบน nodejs.org) | **มือใหม่: ติดตั้งผ่าน installer LTS จาก nodejs.org เป็นหลัก** — อย่าใช้สาย Current (26.x) |
| fnm (Node version manager) | ล่าสุดจาก GitHub Schniz/fnm | **ทางเลือก/ขั้นสูง** (เมื่อเริ่มคล่อง terminal) ใช้สลับเวอร์ชัน Node ได้ ใช้ตัวเดียวกันทั้ง Windows / macOS / Linux |
| Git | **v2.5x** (ติดตั้ง latest) | ติดตั้งจาก git-scm.com / winget / brew จะได้ตัว latest อัตโนมัติ — ไม่ต้อง pin เลขเป๊ะ |
| VS Code | **v1.12x** (เปิด auto-update) | ติดตั้งแล้วเปิด auto-update ทิ้งไว้ — ไม่ต้อง pin เลขเป๊ะ |

> ถ้าผู้เรียนเข้าหน้าเว็บแล้วเห็นเลขเวอร์ชันสูงกว่านี้ ไม่เป็นไร — ให้ติดตั้ง **เวอร์ชัน LTS ล่าสุด** ที่เว็บแนะนำ ณ ตอนนั้นเสมอ.

---

## 1. ตรวจ OS + เปิด Terminal ให้ถูก

### วิธีเดาคร่าวๆ / ถามผู้เรียน
ถามก่อน. ถ้าผู้เรียนไม่แน่ใจ ให้รันคำสั่งเช็ค OS ตามด้านล่าง.

### Windows
- เปิด Terminal: กดปุ่ม Start → พิมพ์ **PowerShell** → คลิก "Windows PowerShell" (หรือ "Terminal" บน Windows 11).
- เช็คเวอร์ชัน Windows: ใน PowerShell พิมพ์
  ```powershell
  winver
  ```
  (เด้งหน้าต่างบอกเวอร์ชัน Windows). ต้องเป็น **Windows 10 หรือ 11** ถึงจะมี `winget`.
- เช็คว่ามี winget ไหม (เครื่องมือติดตั้งโปรแกรมในตัวของ Windows):
  ```powershell
  winget --version
  ```
  ถ้าขึ้นเลขเวอร์ชัน = ใช้ได้. ถ้า error ไปดู Troubleshooting ข้อ 9.1.

### macOS
- เปิด Terminal: กด `Cmd + Space` → พิมพ์ **Terminal** → Enter.
- เช็คเวอร์ชัน macOS:
  ```bash
  sw_vers
  ```
- เช็คว่ามี Homebrew (ตัวจัดการแพ็กเกจยอดนิยมของ Mac):
  ```bash
  brew --version
  ```
  ถ้า error = ยังไม่มี Homebrew → ติดตั้งตามข้อ 2 (macOS) ก่อน.

### Linux (Ubuntu/Debian เป็นหลัก)
- เปิด Terminal: กด `Ctrl + Alt + T`.
- เช็ค distro:
  ```bash
  cat /etc/os-release
  ```

---

## 2. ติดตั้ง Node.js

**ทำไมต้องมี Node.js?** เพราะมันคือตัวรัน JavaScript นอกเบราว์เซอร์ และ Next.js กับเครื่องมือเกือบทั้งหมดต้องรันบน Node.js. เราจะใช้สาย **LTS** (เสถียร เหมาะมือใหม่) เสมอ — อย่าใช้สาย Current (26.x).

### ทางหลักสำหรับมือใหม่ — installer LTS จาก nodejs.org (แนะนำ ผ่านเร็ว เห็นผลไว)

1. ไปที่ <https://nodejs.org/en/download> แล้วเลือก **LTS** (ปุ่ม/แท็บที่เขียน "LTS" — ปัจจุบันคือสาย 24.x).
2. ดาวน์โหลด installer ของ OS ตัวเอง (Windows `.msi` / macOS `.pkg`) แล้วกด **Next ตามค่าเริ่มต้นจนจบ** (Windows ให้แน่ใจว่าติ๊ก "Add to PATH" ซึ่งเป็นค่า default อยู่แล้ว).
   - **ทางเลือกผ่าน package manager** ถ้าผู้เรียนถนัด: Windows `winget install OpenJS.NodeJS.LTS` ; macOS `brew install node@24`.
3. **ปิด/เปิด terminal ใหม่** แล้วเช็คว่าได้สาย LTS:
   ```bash
   node -v   # ต้องขึ้น v24.x.x
   ```
   ถ้าขึ้น v24.x = ผ่าน ไปขั้นถัดไปได้เลย.

> เป้าหมาย Phase 0 คือ "เห็นเว็บรันในวันแรก" — installer ตรงนี้ผ่านเร็วและพลาดยาก ใช้อันนี้ก่อนเสมอสำหรับมือใหม่ล้วน.

### (ทางเลือก/ขั้นสูง) ติดตั้งผ่าน fnm — ทำเมื่อเริ่มคล่อง terminal แล้ว

**ทำไมถึงมีทางนี้?** งานจริงบางครั้งต้องสลับเวอร์ชัน Node ตามโปรเจกต์. `fnm` (Fast Node Manager เขียนด้วย Rust) ทำได้และใช้ได้ทุก OS. แต่ขั้นตอนแก้ไฟล์ profile/shell + ปิดเปิด terminal หลายรอบเป็นจุดที่มือใหม่สะดุดบ่อย — จึงเป็น **ทางเลือก ไม่ใช่ค่าเริ่มต้น**.

> ⏱️ **Fallback สำคัญ:** ถ้าทำ fnm แล้วสะดุดเกิน ~10 นาที (execution policy, profile path, PATH ไม่เจอ) ให้ **ข้ามไปใช้ installer จาก nodejs.org ด้านบนทันที อย่าติดหล่ม** — รักษาโมเมนตัมวันแรกไว้ก่อน. (จะกลับมาลอง fnm ทีหลังตอนคล่องขึ้นก็ได้.)

#### Windows — ติดตั้ง fnm
1. ติดตั้ง fnm ผ่าน winget:
   ```powershell
   winget install Schniz.fnm
   ```
2. **ปิดแล้วเปิด PowerShell ใหม่** (สำคัญ — เพื่อให้ระบบรู้จัก fnm).
3. ตั้งค่าให้ fnm โหลดทุกครั้งที่เปิด PowerShell — เปิดไฟล์ profile:
   ```powershell
   notepad $PROFILE
   ```
   ถ้าเด้งถามว่า "สร้างไฟล์ใหม่ไหม" ให้ตอบ Yes. แล้ว **เพิ่มบรรทัดนี้ต่อท้ายไฟล์** บันทึก แล้วปิด:
   ```powershell
   fnm env --use-on-cd --shell powershell | Out-String | Invoke-Expression
   ```
   > ถ้า notepad เปิดไฟล์ว่างหรือ error เรื่อง path ไปดู Troubleshooting ข้อ 9.2.
4. **ปิดแล้วเปิด PowerShell ใหม่อีกครั้ง** แล้วลงตัว Node LTS:
   ```powershell
   fnm install --lts
   fnm use --lts
   fnm default lts-latest
   ```

#### macOS — ติดตั้ง fnm
1. ถ้ายังไม่มี Homebrew ติดตั้งก่อน:
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```
   (ทำตามที่ terminal บอกตอนจบ — มันอาจให้รัน 2-3 บรรทัดเพิ่มเพื่อเพิ่ม brew เข้า PATH).
2. ติดตั้ง fnm:
   ```bash
   brew install fnm
   ```
3. ตั้งค่าให้ fnm โหลดทุกครั้ง — Mac ใหม่ใช้ **zsh** ให้เพิ่มบรรทัดลง `~/.zshrc`:
   ```bash
   echo 'eval "$(fnm env --use-on-cd --shell zsh)"' >> ~/.zshrc
   ```
   แล้วโหลด config ใหม่:
   ```bash
   source ~/.zshrc
   ```
4. ลง Node LTS:
   ```bash
   fnm install --lts
   fnm use --lts
   fnm default lts-latest
   ```

#### Linux — ติดตั้ง fnm
1. ติดตั้งผ่าน script:
   ```bash
   curl -fsSL https://fnm.vercel.app/install | bash
   ```
2. ปิด/เปิด terminal ใหม่ (script จะเพิ่ม config ลง shell ให้). ถ้าใช้ bash และยังไม่เห็น fnm ให้เพิ่มเอง:
   ```bash
   echo 'eval "$(fnm env --use-on-cd --shell bash)"' >> ~/.bashrc
   source ~/.bashrc
   ```
3. ลง Node LTS:
   ```bash
   fnm install --lts
   fnm use --lts
   fnm default lts-latest
   ```

---

## 3. ติดตั้ง Git

**ทำไม?** Git ใช้เก็บประวัติโค้ด (version control) และเป็นตัวที่ push โค้ดขึ้น GitHub — เป็นทักษะบังคับของงานจริงทุกที่.

### Windows
- ติดตั้งผ่าน winget (ได้ตัวล่าสุดอัตโนมัติ):
  ```powershell
  winget install Git.Git
  ```
- **ปิด/เปิด PowerShell ใหม่** หลังลงเสร็จ.
- (ทางเลือก) ถ้าอยากใช้ installer แบบคลิก ดาวน์โหลดจาก <https://git-scm.com/download/win> แล้วกด Next ตามค่าเริ่มต้นได้เลย.

### macOS
- วิธีง่ายสุด ผ่าน Homebrew:
  ```bash
  brew install git
  ```
- หรือพิมพ์ `git --version` ครั้งแรก macOS อาจเด้งให้ติดตั้ง Xcode Command Line Tools — กดยอมรับก็ได้ Git เช่นกัน.

### Linux (Ubuntu/Debian)
```bash
sudo apt update && sudo apt install git -y
```

### ตั้งค่า Git ครั้งแรก (ทุก OS — ทำครั้งเดียว)
ใช้ชื่อ/อีเมลเดียวกับที่จะสมัคร GitHub:
```bash
git config --global user.name "ชื่อของผู้เรียน"
git config --global user.email "อีเมลของผู้เรียน"
git config --global init.defaultBranch main
```
> ตั้ง default branch เป็น `main` เพราะ GitHub และ Vercel ใช้ `main` เป็นมาตรฐาน.

---

## 4. ติดตั้ง VS Code (editor)

**ทำไม?** เป็น editor ยอดนิยมที่สุดสำหรับ web dev เข้ากับ Next.js/React ได้ดี.

### Windows
```powershell
winget install Microsoft.VisualStudioCode
```
> ระหว่างติดตั้งด้วย installer แบบคลิก ให้ติ๊ก **"Add to PATH"** ด้วย (winget ทำให้อัตโนมัติ).

### macOS
```bash
brew install --cask visual-studio-code
```

### Linux / ทุก OS แบบโหลดเอง
ดาวน์โหลดจาก <https://code.visualstudio.com/download> เลือกตาม OS.

### เปิด VS Code จาก terminal ได้ (ทุก OS)
ทดสอบว่า command `code` ใช้ได้:
```bash
code --version
```
- ถ้า **macOS** แล้ว `code` ใช้ไม่ได้: เปิด VS Code → กด `Cmd+Shift+P` → พิมพ์ `Shell Command: Install 'code' command in PATH` → Enter.

### Extensions ที่แนะนำให้ลง (ผ่านแถบ Extensions รูปสี่เหลี่ยมด้านซ้าย)
- **ESLint** (จับ error โค้ด)
- **Prettier - Code formatter** (จัดหน้าโค้ดให้สวยอัตโนมัติ)
- **Tailwind CSS IntelliSense** (เติม class Tailwind อัตโนมัติ — ใช้หนักตั้งแต่ Phase 1)
- **Prisma** (ใช้ตอน Phase 4)

---

## 5. สมัคร GitHub

**ทำไม?** ใช้เก็บโค้ดบน cloud, เป็น portfolio ให้คนอื่น/บริษัทดูได้, และเชื่อมกับ Vercel เพื่อ deploy.

1. ไปที่ <https://github.com/signup> สมัครด้วยอีเมล (แนะนำใช้อีเมลเดียวกับที่ตั้งใน `git config`).
2. ตั้ง username ดีๆ (จะกลายเป็น URL portfolio เช่น `github.com/username`).
3. ยืนยันอีเมล.
4. **เชื่อม Git ในเครื่องกับ GitHub** — วิธีง่ายสุดสำหรับมือใหม่คือใช้ GitHub CLI:
   - ติดตั้ง:
     - Windows: `winget install GitHub.cli`
     - macOS: `brew install gh`
     - Linux: ดู <https://github.com/cli/cli#installation>
   - **ปิด/เปิด terminal ใหม่** แล้ว login:
     ```bash
     gh auth login
     ```
     เลือก: `GitHub.com` → `HTTPS` → ตอบ Yes ให้ใช้ credentials กับ git → `Login with a web browser` → ทำตามขั้นตอนในเบราว์เซอร์.
   > วิธีนี้เลี่ยงเรื่อง SSH key/personal access token ที่ทำให้มือใหม่งงได้.

---

## 6. สมัคร Vercel

**ทำไม?** เป็นที่ deploy เว็บ Next.js ฟรีและง่ายที่สุด (Next.js กับ Vercel เป็นเจ้าของเดียวกัน). ใช้จริงตอน Phase 8 แต่สมัครรอไว้ตั้งแต่ Phase 0 ได้.

1. ไปที่ <https://vercel.com/signup>
2. กด **"Continue with GitHub"** (สมัครด้วยบัญชี GitHub ที่เพิ่งสร้าง — จะทำให้ deploy ทีหลังง่ายมาก เพราะ Vercel เห็น repo ได้เลย).
3. อนุญาต (authorize) ให้ Vercel เข้าถึง GitHub ตามที่หน้าจอบอก.
4. เลือกแพลน **Hobby (ฟรี)** — พอสำหรับโปรเจกต์เรียนและ portfolio.

> ยังไม่ต้อง deploy ตอนนี้ แค่มีบัญชีพร้อม. รายละเอียด deploy อยู่ใน Phase 8.

---

## 7. เช็คว่าติดตั้งสำเร็จครบ (checklist ก่อนสร้างโปรเจกต์)

ให้ผู้เรียนรันทีละบรรทัดใน terminal แล้ว Claude ตรวจว่าทุกอันขึ้นเลขเวอร์ชัน (ไม่ใช่ error):

```bash
node -v      # ต้องขึ้น v24.x.x (สาย LTS) — load-bearing ต้องเป็น v24.x
npm -v       # ต้องขึ้นเลขเวอร์ชัน (มาพร้อม Node อัตโนมัติ)
git --version    # ต้องขึ้น git version 2.5x.x
code --version   # ต้องขึ้นเลขเวอร์ชัน VS Code (สาย 1.12x)
gh --version     # (ถ้าใช้ GitHub CLI) ต้องขึ้นเลขเวอร์ชัน
fnm -V           # (เฉพาะคนที่เลือกติดตั้ง fnm) ต้องขึ้นเลขเวอร์ชัน fnm
```

ผ่านเกณฑ์เมื่อ: `node -v` ขึ้น **v24.x**, npm/git/code ขึ้นเวอร์ชันครบ, มีบัญชี GitHub + Vercel แล้ว.

> ถ้า `node -v` ขึ้นเป็นสาย **26.x** (Current): ถ้าใช้ installer ให้ถอนแล้วลง **LTS** จาก nodejs.org ใหม่; ถ้าใช้ fnm ให้สลับกลับ LTS: `fnm install --lts && fnm default lts-latest` แล้วปิด/เปิด terminal ใหม่.

---

## 8. สร้างโปรเจกต์ Next.js แรก (TaskFlow)

**ทำไมใช้ `create-next-app`?** เป็นตัวสร้างโครงโปรเจกต์ Next.js อย่างเป็นทางการ — ตั้งค่าทุกอย่างให้พร้อมรันทันที.

1. เลือกโฟลเดอร์เก็บงาน เช่น:
   ```bash
   cd ~/Desktop
   ```
   (Windows PowerShell ใช้ `cd $HOME\Desktop`)
2. สร้างโปรเจกต์:
   ```bash
   npx create-next-app@latest taskflow
   ```
3. ตอบคำถาม (prompt) ของ `create-next-app`. **prompt เป็นภาษาอังกฤษทั้งหมด และตอบผิดแม้ข้อเดียว (โดยเฉพาะ TypeScript) จะกระทบทั้งหลักสูตร** — จุดนี้คือจุดที่มือใหม่พลาดบ่อยที่สุดในวันแรก. ทำตามนี้:

   > 🧑‍🏫 **คำสั่งสำหรับ Claude ผู้สอน (ทำแบบ interactive — สำคัญมาก):** **อย่าปล่อยให้ผู้เรียนอ่าน prompt อังกฤษแล้วกดตอบเอง.** ให้ผู้เรียนพิมพ์คำสั่ง `npx create-next-app@latest taskflow` แล้ว **หยุด** — ให้ผู้เรียน **อ่านบรรทัด prompt ที่ขึ้นบนจอให้ผู้สอนฟังทีละข้อ** (หรือก๊อปข้อความมาวาง) แล้วผู้สอน **บอกว่าให้กดตอบอะไร ทีละข้อ** จนครบ. เหตุผล: prompt อาจขึ้น **ไม่เรียงตามลำดับในเอกสารนี้** และบางเวอร์ชันถาม/ไม่ถามบางข้อ — ถ้าผู้เรียนกดเองตามลำดับในเอกสารจะพลาดง่าย.
   >
   > ⛔ **ถ้าหน้าตา prompt ที่ขึ้นบนจอไม่ตรงกับตารางด้านล่าง (เจอคำถามที่ไม่มีในตาราง หรือข้อความต่างไป) ให้หยุดถามผู้สอนก่อน อย่าเดาคำตอบ.**

   **ตารางจับคู่ "คำถามภาษาอังกฤษ → ตอบอะไร" (ยึดความหมายของคำถาม ไม่ใช่ลำดับ):**

   | คำถามที่ขึ้นบนจอ (อังกฤษ) | กดตอบ | ทำไม |
   |---|---|---|
   | `Would you like to use the recommended Next.js defaults?` | **No, customize settings** | ⛔ ถ้าตอบ "Yes, use recommended defaults" มันจะบังคับ **TypeScript = Yes** ทันที ขัดกับหลักสูตรที่เริ่มด้วย JavaScript ก่อน |
   | `Would you like to use TypeScript?` | **No** | เริ่ม JavaScript ก่อน ค่อยเพิ่ม TypeScript ตอน Phase 4 |
   | `Which linter would you like to use?` | **ESLint** | (มีตัวเลือก Biome ด้วย แต่หลักสูตรนี้ใช้ ESLint) |
   | `Would you like to use React Compiler?` | **No** | ยังไม่ใช้ในหลักสูตร |
   | `Would you like to use Tailwind CSS?` | **Yes** | ใช้แต่งหน้าตาตั้งแต่ Phase 1 |
   | `Would you like your code inside a \`src/\` directory?` | **Yes** | โครงโปรเจกต์เป็นระเบียบ |
   | `Would you like to use App Router? (recommended)` | **Yes** | ⛔ สำคัญ — ทั้งหลักสูตรใช้ App Router |
   | `Would you like to customize the import alias (\`@/*\` by default)?` | **No** | ใช้ค่าเริ่มต้น `@/*` |
   | `Would you like to include an AGENTS.md file?` | **Yes** | ช่วยให้ผู้สอน/coding agent เขียนโค้ดได้ตรงเวอร์ชัน |

   > วิธีกดตอบ prompt: ใช้ลูกศร ⬆️⬇️ เลื่อนเลือกตัวเลือก หรือพิมพ์ `y`/`n` แล้วกด **Enter**. ตัวเลือกที่ถูกไฮไลต์อยู่คือตัวที่จะถูกเลือกเมื่อกด Enter — ดูให้ตรงก่อนกดทุกครั้ง.
   > หมายเหตุ: Next.js 16 **ไม่ถาม "Turbopack? Yes/No" แล้ว** เพราะ Turbopack เป็นค่า default ในตัว (ถ้าจำเป็นต้องปิดต้องใช้ flag `--webpack` — ปกติไม่ต้องทำ).
   > หลักการกันพลาด ถ้าเวอร์ชันถามไม่ครบ/เพิ่มข้อ: **JavaScript (TypeScript = No), ESLint, Tailwind = Yes, src = Yes, App Router = Yes** คือค่าที่ต้องได้เสมอ.
   >
   > 📄 **ไฟล์ `AGENTS.md` คืออะไร (เมื่อตอบ Yes ข้อสุดท้าย):** `create-next-app` จะ generate ไฟล์ `AGENTS.md` ที่ root โปรเจกต์ให้. มันคือไฟล์ "คำแนะนำสำหรับ coding agent" ตามมาตรฐานกลาง (open standard) ที่หลายเครื่องมือ AI อ่านได้ ภายในเป็นโน้ตเรื่อง convention/คำสั่งของโปรเจกต์ Next.js. **ต่างจาก `CLAUDE.md` ตรงที่** `CLAUDE.md` เป็นไฟล์เฉพาะของ Claude Code (เครื่องมือที่เราใช้สอน) ส่วน `AGENTS.md` เป็นรูปแบบกลางที่ไม่ผูกกับเครื่องมือใดเครื่องมือหนึ่ง — เนื้อหาคล้ายกัน ต่างแค่ว่าใครอ่าน. **ผู้เรียนไม่ต้องทำอะไรกับมัน:** หลักสูตรนี้ใช้ `CLAUDE.md` เป็นหลักอยู่แล้ว จะเก็บ `AGENTS.md` ไว้เฉยๆ (ไม่กระทบอะไร) หรือจะลบทิ้งก็ได้ตามสะดวก.
4. เข้าโฟลเดอร์แล้วรัน:
   ```bash
   cd taskflow
   npm run dev
   ```
5. เปิดเบราว์เซอร์ไปที่ <http://localhost:3000> — ต้องเห็นหน้า Next.js เริ่มต้น. หยุดเซิร์ฟเวอร์ด้วย `Ctrl + C`.
6. เปิดโปรเจกต์ใน VS Code:
   ```bash
   code .
   ```
   > 💡 หลัง `code .` ให้เปิด **integrated terminal ใน VS Code** (เมนู **Terminal → New Terminal** หรือกด `` Ctrl+` ``) แล้วใช้เทอร์มินัลนี้สำหรับคำสั่ง `git` ทั้งหมด — โดยเฉพาะเมื่อเทอร์มินัลเดิมยังติดรัน dev server อยู่ จะได้ไม่สับสนว่าควรพิมพ์คำสั่งในหน้าต่างไหน.

### เชื่อมกับ GitHub (push ครั้งแรก)
`create-next-app` ทำ `git init` ให้แล้ว เหลือสร้าง repo บน GitHub แล้ว push. ถ้าใช้ GitHub CLI ง่ายสุด:
```bash
git add .
git commit -m "Initial commit: TaskFlow project setup"
gh repo create taskflow --public --source=. --remote=origin --push
```
> ถ้าไม่ใช้ `gh`: ไปสร้าง repo เปล่าชื่อ `taskflow` บน github.com (อย่าติ๊ก add README) แล้วทำตามคำสั่ง "push an existing repository" ที่ GitHub โชว์ให้.

หลังจบขั้นนี้ = จบ Phase 0. พร้อมเข้า Phase 1.

---

## 8.5 React Developer Tools (browser extension — ใช้ใน Mini-Phase A หลัง Phase 2)

**ทำไม?** Mini-Phase A (Debugging & DevTools) สอนให้ส่อง state/props ของ React component. DevTools ที่ติดมากับเบราว์เซอร์เห็นแค่ DOM (HTML สุดท้าย) แต่ **ไม่เห็นโลกของ React** — ต้องลง extension เสริม "React Developer Tools" จึงจะมีแท็บ **Components** / **Profiler** ให้ใช้.

> ติดตั้ง **ตอนถึง Mini-Phase A** (หลัง Phase 2) ไม่ต้องลงใน Phase 0. เป็น extension ฟรีอย่างเป็นทางการจาก Meta (React team).

- **Chrome / Edge / Brave (เบราว์เซอร์ตระกูล Chromium):** เปิด **Chrome Web Store** ค้นคำว่า **"React Developer Tools"** (ผู้พัฒนา: Meta/Facebook) → กด **Add to Chrome / เพิ่มลงใน Chrome**.
- **Firefox:** เปิด **Firefox Add-ons** (addons.mozilla.org) ค้น **"React Developer Tools"** → **Add to Firefox**.
- หลังติดตั้งเสร็จ **รีเฟรชหน้า TaskFlow (`localhost:3000`) 1 ครั้ง** แล้วเปิด DevTools (F12) จะเห็นแท็บ **Components** เพิ่มขึ้นมา.

> 💡 ผู้สอนยืนยันชื่อ store / ขั้นตอนตามเบราว์เซอร์ที่ผู้เรียนใช้จริงก่อนพา (Chromium ใช้ Chrome Web Store ได้หมด). **ห้าม pin เลขเวอร์ชัน** — ติดตั้งรุ่นล่าสุดจาก store เสมอ. ถ้าแท็บ Components ไม่โผล่ ให้ปิด/เปิดเบราว์เซอร์แล้วรีเฟรชหน้าอีกครั้ง.

---

## 9. Troubleshooting (ปัญหามือใหม่เจอบ่อย)

### 9.1 `winget` ใช้ไม่ได้ (Windows)
- อัปเดต **App Installer** จาก Microsoft Store. หรือลง Windows updates ให้ครบ.
- ทางเลือก: ลงโปรแกรมจาก installer แบบคลิกแทน (Node จาก nodejs.org, Git จาก git-scm.com, VS Code จาก code.visualstudio.com).

### 9.2 `notepad $PROFILE` เปิดไม่ได้ / fnm ยังไม่ทำงาน (Windows)
- สร้างโฟลเดอร์ profile ก่อน:
  ```powershell
  if (!(Test-Path $PROFILE)) { New-Item -ItemType File -Path $PROFILE -Force }
  notepad $PROFILE
  ```
- ใส่บรรทัด fnm env (ข้อ 2) ให้ครบ บันทึก แล้ว **ปิด/เปิด PowerShell ใหม่**.
- ถ้า PowerShell บล็อกการรัน script (error เรื่อง execution policy) รัน:
  ```powershell
  Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
  ```
  ตอบ `Y` แล้วลองใหม่.

### 9.3 พิมพ์ `node`/`git`/`code` แล้วขึ้น "not recognized" / "command not found"
- **เกือบทุกครั้งแก้ได้ด้วยการปิด terminal แล้วเปิดใหม่** (PATH เพิ่งอัปเดตหลังติดตั้ง).
- ยังไม่หาย: รีสตาร์ตเครื่อง 1 ครั้ง.
- `code` บน macOS: ใช้ `Shell Command: Install 'code' command in PATH` (ดูข้อ 4).

### 9.4 `node -v` ขึ้นเวอร์ชันผิด / ไม่ใช่ LTS
```bash
fnm install --lts
fnm use --lts
fnm default lts-latest
```
ปิด/เปิด terminal ใหม่ แล้วเช็ค `node -v` อีกครั้ง.

### 9.5 `npm run dev` แล้ว error ว่า port 3000 ถูกใช้แล้ว ("address already in use")
- มี dev server ค้างอยู่. ปิดด้วย `Ctrl + C` ในหน้าต่างเดิม หรือรันที่ port อื่น:
  ```bash
  npm run dev -- -p 3001
  ```
  แล้วเปิด <http://localhost:3001>.

### 9.6 `npx create-next-app` ค้าง/ช้ามาก หรือ error เรื่อง network
- เช็คเน็ต. ลองใหม่อีกครั้ง (บางทีติด proxy/firewall).
- ถ้าค้างนานตอน install ให้ `Ctrl + C` ลบโฟลเดอร์ที่สร้างไม่เสร็จแล้วรันใหม่.

### 9.7 `gh auth login` หรือ `gh repo create` ใช้ไม่ได้
- ยังไม่ได้ login: รัน `gh auth login` ก่อน (ดูข้อ 5).
- ไม่อยากใช้ `gh`: สร้าง repo บน github.com เองแล้ว push ตามคำสั่งที่ GitHub โชว์ (ดูหมายเหตุข้อ 8).

### 9.8 macOS: `brew` ใช้ไม่ได้หลังติดตั้ง
- ทำตามบรรทัด "Next steps" ที่ Homebrew พิมพ์ตอนจบติดตั้ง (มันให้รัน 2 คำสั่งเพิ่ม PATH). ปิด/เปิด Terminal ใหม่.

### 9.9 ติดตั้งต้องใช้สิทธิ์ admin / permission denied
- **Windows**: เปิด PowerShell โดยคลิกขวา → "Run as administrator" เฉพาะตอนติดตั้งโปรแกรม.
- **macOS/Linux**: คำสั่งที่ขึ้นต้นด้วย `sudo` จะถามรหัสผ่านเครื่อง — พิมพ์รหัสได้เลย (จอจะไม่โชว์ตัวอักษร เป็นเรื่องปกติ).

### 9.10 Auth.js (Phase 5): ใช้ `proxy.ts` ไม่ใช่ `middleware.ts` (Next.js 16)
- บน **Next.js 16** ไฟล์ `middleware.ts` ถูกเปลี่ยนชื่อเป็น **`proxy.ts`** แล้ว. สำหรับ Auth.js v5 ที่ต้องทำ protected route ให้สร้างไฟล์ **`proxy.ts`** (ระดับ root หรือใน `src/`) แทน เช่น:
  ```ts
  export { auth as middleware } from "@/auth"
  ```
  (โค้ดด้านในยัง `export { auth as middleware }` เหมือนเดิม — เปลี่ยนแค่ **ชื่อไฟล์** จาก `middleware.ts` เป็น `proxy.ts`).
- ⚠️ **เตือนผู้สอน:** ตัวอย่าง Auth.js v5 ส่วนใหญ่ในเน็ต (เขียนสำหรับ Next 14/15) ยังใช้ `middleware.ts` — ถ้าวางตามนั้นบน Next.js 16 จะ **ไม่ทำงานและ debug ยากมากสำหรับมือใหม่**. ต้องเปลี่ยนเป็น `proxy.ts` เสมอ.

---

## 9.5 คู่มือ error ที่เจอบ่อยตามเฟส (Phase 1–8)

> ส่วนนี้คือ error เชิง **โค้ด/เฟรมเวิร์ก** ที่ผู้เรียนเริ่มเจอตั้งแต่ Phase 1 ขึ้นไป (ต่างจาก §9 ที่เป็น error ตอนติดตั้งเครื่องมือ). วิธีใช้สำหรับผู้สอน: เจอ error → จับคู่ "อาการ" ด้านล่าง → พาแก้ทีละขั้น → ถ้า docs ของเครื่องมือเปลี่ยนไป ให้เปิด docs ที่ลิงก์ไว้เช็คก่อน.

### (ก) Hydration error / mismatch (React 19 + Next.js) — เริ่มเจอตั้งแต่ Phase 1–3
- **อาการ:** คอนโซลขึ้น `Hydration failed because the server rendered HTML didn't match the client` หรือ `Text content does not match server-rendered HTML`. หน้าจออาจกระพริบหรือ render ซ้ำ.
- **สาเหตุ:** HTML ที่ render ฝั่ง server **ไม่ตรง** กับฝั่ง client. ตัวการยอดฮิตของมือใหม่: อ่านค่าที่ "มีเฉพาะบน browser" ตอน render ตรงๆ เช่น `localStorage`, `window`, `Date.now()`, `Math.random()` — ฝั่ง server ไม่มีค่าพวกนี้ จึงได้ผลต่างกัน.
- **วิธีแก้ (สั้นๆ):**
  1. **เช็คก่อนเลยว่าอ่าน `localStorage` (หรือ `window`) ใน `useEffect` หรือยัง** — ของพวกนี้ต้องอ่าน **ใน `useEffect`** (รันเฉพาะฝั่ง client หลัง mount) ไม่ใช่อ่านตอน render ตรงๆ. เริ่ม state เป็นค่าว่าง/ค่า default ก่อน แล้วค่อย `setState` จากค่าใน `localStorage` ภายใน `useEffect`.
  2. ค่าที่เปลี่ยนทุกครั้ง (`Date.now()`, `Math.random()`) ก็ย้ายไป `useEffect` เช่นกัน หรือคำนวณนอก render.
  3. ตรวจ HTML ที่ผิดมาตรฐาน เช่น `<p>` ซ้อน `<div>` (ทำให้ browser แก้ DOM เอง → mismatch).
- **docs:** Next.js — <https://nextjs.org/docs/messages/react-hydration-error>

### (ข) `Module not found` / import path ผิด — เจอบ่อยตอนย้าย component (Phase 2 ขึ้นไป)
- **อาการ:** `Module not found: Can't resolve './TaskItem'` หรือ `Cannot find module '@/components/...'` หลังจากย้าย/เปลี่ยนชื่อไฟล์.
- **สาเหตุ:** path ใน `import` ไม่ตรงกับตำแหน่งไฟล์จริง — มัก import ด้วย relative path (`../`) ที่ค้างจากตำแหน่งเดิม, สะกด/ตัวพิมพ์เล็กใหญ่ไม่ตรง (สำคัญบน Mac/Linux), หรือลืมเปลี่ยน path หลังลากไฟล์ใน VS Code.
- **วิธีแก้ (สั้นๆ):**
  1. ใช้ **import alias `@/`** (ตั้งไว้ตอน §8) ชี้จาก `src/` แทน relative path ยาวๆ เช่น `import TaskItem from "@/components/TaskItem"` — ย้ายไฟล์แล้ว path พังยากกว่า.
  2. เช็คชื่อไฟล์ + ตัวพิมพ์ให้ตรงเป๊ะ (`TaskItem.jsx` ≠ `taskitem.jsx`).
  3. หลังย้ายไฟล์ ให้ VS Code ช่วยอัปเดต import อัตโนมัติ (กดยอมรับเมื่อ VS Code ถาม "Update imports?") และลอง **restart dev server** หนึ่งครั้งถ้า cache ค้าง.
- **docs:** Next.js (import alias) — <https://nextjs.org/docs/app/getting-started/installation#set-up-absolute-imports-and-module-path-aliases>

### (ค) Prisma: `Client not generated` + migration ค้าง / schema drift (Phase 4 ขึ้นไป)
- **อาการ 1 (client):** `@prisma/client did not initialize yet. Please run "prisma generate"` หรือ type/field ใหม่ที่เพิ่งเพิ่มใน schema เรียกใช้ไม่ได้.
  - **แก้:** รัน `npx prisma generate` แล้ว **restart dev server**. (ทุกครั้งที่แก้ `schema.prisma` ควร generate ใหม่ — ปกติ `prisma migrate dev` generate ให้อยู่แล้ว.)
- **อาการ 2 (migration ค้าง / schema drift):** ตอนรัน `npx prisma migrate dev` ขึ้นเตือน `Drift detected: Your database schema is not in sync with your migration history` หรือถามให้ reset.
  - **สาเหตุ:** ฐานข้อมูลจริงกับ migration history ไม่ตรงกัน (เช่นแก้ DB มือ, ลบ/แก้ไฟล์ migration, หรือ migration ก่อนหน้าทำไม่จบ).
  - **แก้ (dev เท่านั้น ข้อมูลทดสอบหายได้):** ทางง่ายสุดคือ `npx prisma migrate reset` (ลบ DB แล้วรัน migration ทั้งหมดใหม่จากต้น) เหมาะกับ `dev.db` ที่เป็นข้อมูลทดลอง. จากนั้นแก้ schema ให้เรียบร้อยแล้ว `npx prisma migrate dev --name <ชื่อ>` ใหม่.
  - ⛔ **อย่าใช้ `migrate reset` กับ DB จริง/production** (ข้อมูลหายหมด) — บน Postgres ตอน deploy ใช้ `npx prisma migrate deploy` แทน (ดู §15).
- **docs:** Prisma — <https://www.prisma.io/docs/orm/prisma-migrate>

### (ง) Server Action error (Next.js App Router) — Phase 4–6
- **อาการ A:** `Functions cannot be passed directly to Client Components unless you explicitly expose it by marking it with "use server"`.
  - **สาเหตุ:** ส่งฟังก์ชันธรรมดาเป็น prop ข้ามจาก Server ไป Client Component โดยฟังก์ชันนั้นไม่ใช่ Server Action.
  - **แก้:** ถ้าตั้งใจให้เป็น Server Action ให้ใส่ directive **`"use server"`** (บรรทัดบนสุดของไฟล์ action หรือบนสุดในตัวฟังก์ชัน async) แล้วค่อยส่งเป็น prop / เรียกจากฟอร์ม. ถ้าไม่ได้ตั้งใจให้ข้าม boundary ให้ย้าย logic ไปฝั่งที่ถูกต้องแทน.
- **อาการ B:** action ที่เขียนไว้ "ไม่ทำงาน/ไม่ถูกเรียก" หรือ error ว่า action ไม่ valid.
  - **สาเหตุยอดฮิต:** **ลืมใส่ `"use server"`** ในไฟล์/ฟังก์ชัน action, หรือใส่ผิดที่.
  - **แก้:** ไฟล์ที่รวม Server Actions ใส่ `"use server"` บรรทัดแรกสุดของไฟล์; ฟังก์ชันต้องเป็น `async`; ฝั่งที่เป็น Client Component อย่าลืม `"use client"` บรรทัดแรกเช่นกัน (คนละ directive กัน — อย่าสลับ).
- **docs:** Next.js — <https://nextjs.org/docs/app/getting-started/updating-data> · directives <https://nextjs.org/docs/app/api-reference/directives/use-server>

### (จ) Auth.js: session เป็น `null` / redirect loop (Phase 5)
- **อาการ A — `session` เป็น `null` ทั้งที่ login แล้ว:** เรียก `auth()` / `useSession()` แล้วได้ `null` ตลอด.
  - **สาเหตุ + แก้ (เช็คตามลำดับ):**
    1. **ลืม/ตั้ง `AUTH_SECRET` ไม่ถูก** — ต้องมีใน `.env` (รัน `npx auth secret`, ดู §12). หลังแก้ `.env` ต้อง **restart dev server** ทุกครั้ง (Next อ่าน env ตอน start).
    2. ไม่มี **route handler** ที่ `src/app/api/auth/[...nextauth]/route.ts` (ดู §12 ข้อ 4) → session ทำงานไม่ครบ.
    3. ฝั่ง Client ที่ใช้ `useSession()` ต้องครอบด้วย `<SessionProvider>`; ฝั่ง Server ใช้ `await auth()` แทน.
- **อาการ B — redirect loop (เด้งไป `/login` วนไม่จบ):** เปิดหน้า protected แล้วเด้งกลับหน้า login ซ้ำๆ จน browser ฟ้อง `too many redirects`.
  - **สาเหตุยอดฮิต:** `proxy.ts` (Next.js 16 — ดู §9.10) ตั้ง matcher ให้ป้องกัน **รวมหน้า `/login` เอง** → ยังไม่ login ก็โดนเด้งไป `/login` ซึ่งก็ถูกป้องกันอีก → วน.
  - **แก้:** ตั้ง `matcher` ใน config ของ proxy ให้ **ยกเว้นหน้า public** (เช่น `/login`, `/signup`, และ path ของ Auth.js เอง `/api/auth/*`). ตรวจ logic ใน callback `authorized`/redirect ว่าไม่ได้ส่งคนที่ยังไม่ login กลับเข้า loop.
- **docs:** Auth.js v5 — <https://authjs.dev> (session: <https://authjs.dev/getting-started/session-management/get-session>)

---

## 10. สรุปสำหรับ Claude เครื่องปลายทาง
- ตรวจ OS → เลือกชุดคำสั่งเดียว → ติดตั้งทีละตัว → เช็คด้วยคำสั่งในข้อ 7 → ยืนยันก่อนไปต่อ.
- ลำดับติดตั้ง: **Node (installer LTS จาก nodejs.org เป็นหลัก; fnm เป็นทางเลือกขั้นสูง) → Git + ตั้งค่า → VS Code + extensions → GitHub (+gh login) → Vercel → create-next-app (TaskFlow) → push GitHub.**
- เกณฑ์จบ Phase 0: `node -v` = v24.x, รัน `npm run dev` เห็นหน้าเว็บที่ localhost:3000, และโค้ดขึ้น GitHub แล้ว.
- เวอร์ชันให้ยึด "LTS ล่าสุด" เสมอ ตารางในข้อ 0 คือค่าอ้างอิง ณ มิ.ย. 2026.
- **§11–§15 ด้านล่างคือการติดตั้งเครื่องมือของ Phase 4 ขึ้นไป** (Prisma, Auth.js, Vitest, Playwright, Postgres) — ทำ "ตอนถึงเฟสนั้น" ไม่ใช่ทำใน Phase 0.

---

> # ⚠️ อ่านก่อนใช้ §11–§15 (เครื่องมือของ Phase 4 ขึ้นไป) — สำคัญมากสำหรับผู้สอน
>
> เครื่องมือกลุ่มนี้ (Prisma, Auth.js, Vitest, Playwright, Neon/Supabase) **ออกเวอร์ชันใหม่ + เปลี่ยนวิธีติดตั้ง/คำสั่งบ่อยกว่า** เครื่องมือใน Phase 0 มาก. คำสั่งด้านล่าง verify ณ **มิถุนายน 2026** และเขียนสำหรับ stack **Next.js 16 + Prisma 7 + Auth.js v5** แต่:
>
> 1. **ตอนถึงเฟสจริง ผู้สอน (Claude) ต้องเปิด docs ทางการเช็คคำสั่ง + prompt ล่าสุดก่อนเสมอ** แล้วค่อยพาผู้เรียนพิมพ์ — ห้ามให้ผู้เรียนพิมพ์ตามนี้ดื้อๆ ถ้า docs เปลี่ยนไปแล้ว. docs ทางการ:
>    - Prisma: <https://www.prisma.io/docs>
>    - Auth.js v5: <https://authjs.dev>
>    - Vitest: <https://vitest.dev> · Next.js testing guide: <https://nextjs.org/docs/app/guides/testing>
>    - Playwright: <https://playwright.dev>
>    - Neon: <https://neon.tech/docs> · Supabase: <https://supabase.com/docs>
> 2. **ทำแบบ interactive เหมือน §8:** ให้ผู้เรียนรันทีละคำสั่ง → อ่าน output/prompt ที่ขึ้นจอให้ผู้สอนฟัง → ผู้สอนบอกตอบ → ยืนยันผลก่อนไปต่อ. **prompt ของ CLI กลุ่มนี้เป็นภาษาอังกฤษทั้งหมด** เช่นกัน.
> 3. ทุกแพ็กเกจใช้ **`@latest`** เพื่อได้รุ่นล่าสุดของ major ที่หลักสูตรล็อกไว้ (เลขเวอร์ชันจริงดูตารางใน LEARNING-PLAN.md / รันคำสั่งเช็คหลังติดตั้ง).
> 4. **รันคำสั่งทั้งหมดในโฟลเดอร์โปรเจกต์ `taskflow`** (ที่มีไฟล์ `package.json`) ผ่าน integrated terminal ใน VS Code.

---

## 11. Prisma + SQLite (Phase 4 — Database)

**ทำไม?** Phase 4 ย้ายข้อมูลจาก localStorage ไป **database จริง**. Prisma = ORM ที่คุยกับ DB ด้วยโค้ด JS/TS; SQLite = DB ไฟล์เดียว ตั้งค่าง่าย เหมาะ dev.

> 📋 ก่อนเริ่ม: เปิดโปรเจกต์ `taskflow` ใน VS Code และอยู่ในโฟลเดอร์โปรเจกต์ใน terminal. (ลง VS Code extension **Prisma** ไว้ตั้งแต่ §4 จะช่วยเรื่อง syntax highlight + format ไฟล์ schema.)

1. **ติดตั้ง Prisma (เป็น dev dependency) + ตัว client ที่โค้ดเรียกใช้:**
   ```bash
   npm install prisma --save-dev
   npm install @prisma/client
   ```
2. **init Prisma โดยตั้ง datasource เป็น SQLite:**
   ```bash
   npx prisma init --datasource-provider sqlite
   ```
   - คำสั่งนี้สร้างโฟลเดอร์ `prisma/` พร้อมไฟล์ `prisma/schema.prisma` และเพิ่ม `DATABASE_URL` ลงไฟล์ `.env` ให้อัตโนมัติ (ค่าเริ่มต้นชี้ไปไฟล์ `file:./dev.db`).
   - **ยืนยันกับผู้เรียน:** เปิดดูว่ามีไฟล์ `prisma/schema.prisma` และ `.env` โผล่ขึ้นมาจริง.
3. **ตรวจว่า `.env` จะไม่ขึ้น git:** create-next-app ใส่ `.env*` ใน `.gitignore` ให้แล้ว — ยืนยันว่ามีบรรทัดนี้ (กันความลับ/ไฟล์ DB หลุดขึ้น GitHub).
4. **เขียน model แรกใน `prisma/schema.prisma`** (Phase 4 จะพาเขียนทีละ field) ตัวอย่าง model `Task`:
   ```prisma
   model Task {
     id        String   @id @default(cuid())
     title     String
     done      Boolean  @default(false)
     createdAt DateTime @default(now())
   }
   ```
5. **สร้างตารางจริงด้วย migration แรก** (ตั้งชื่อ migration ได้ตามใจ เช่น `init`):
   ```bash
   npx prisma migrate dev --name init
   ```
   - คำสั่งนี้สร้างไฟล์ DB `prisma/dev.db`, สร้างตาราง, และ generate Prisma Client ให้พร้อมใช้ในโค้ด.
6. **เปิด Prisma Studio ดูตารางจริง** (หน้าเว็บแก้ข้อมูลใน DB — ดีมากสำหรับมือใหม่ให้เห็นภาพ DB):
   ```bash
   npx prisma studio
   ```
   เปิดที่ <http://localhost:5555>. หยุดด้วย `Ctrl + C`.

> 🔁 **ทุกครั้งที่แก้ `schema.prisma` ภายหลัง** (เช่น Phase 5 เพิ่ม `User`, Phase 6 เพิ่ม `category`) ให้รัน `npx prisma migrate dev --name <ชื่อสื่อความหมาย>` อีกครั้งเพื่ออัปเดตตาราง.
> ⚠️ Prisma 7: ถ้า client ใช้ไม่ได้/type ไม่อัปเดตหลังแก้ schema ให้รัน `npx prisma generate` ซ้ำ แล้ว restart dev server.

---

## 12. Auth.js v5 (Phase 5 — Auth) + env / secret

**ทำไม?** Phase 5 ทำระบบสมาชิก. Auth.js v5 (แพ็กเกจ `next-auth` สาย v5) จัดการ login/session ให้.

> ⚠️ **อ่าน §9.10 (proxy.ts) ก่อน** — บน Next.js 16 ต้องใช้ไฟล์ **`proxy.ts`** ไม่ใช่ `middleware.ts`. ตัวอย่าง Auth.js ในเน็ตส่วนใหญ่ยังเป็น `middleware.ts` (Next 14/15) ต้องเปลี่ยนเสมอ.

1. **ติดตั้ง Auth.js:**
   ```bash
   npm install next-auth@beta
   ```
   > หมายเหตุ: Auth.js v5 ยัง publish ภายใต้ tag `next-auth@beta` ณ มิ.ย. 2026. **ผู้สอนเช็ค <https://authjs.dev> ก่อนเสมอ** เผื่อเปลี่ยนเป็น stable tag แล้ว.
2. **สร้าง `AUTH_SECRET` (ความลับสำหรับเซ็น session) ใส่ลง `.env`:**
   ```bash
   npx auth secret
   ```
   - คำสั่งนี้สุ่มค่า secret แล้วเขียน `AUTH_SECRET=...` ลงไฟล์ `.env` ให้อัตโนมัติ.
   - อธิบายผู้เรียน: นี่คือ **ความลับ ห้าม commit ขึ้น git** (อยู่ใน `.env` ซึ่ง gitignore ไว้แล้ว). ตอน deploy (Phase 8) ต้องไปตั้งค่านี้บน Vercel แยกต่างหาก.
3. **สร้างไฟล์ config หลัก `src/auth.ts`** (Phase 5 จะพาเขียนเต็ม) โครงเริ่มต้นแบบ **Credentials provider** (อีเมล+รหัสผ่าน):
   ```ts
   import NextAuth from "next-auth"
   import Credentials from "next-auth/providers/credentials"

   export const { handlers, auth, signIn, signOut } = NextAuth({
     providers: [
       Credentials({
         credentials: { email: {}, password: {} },
         authorize: async (credentials) => {
           // Phase 5: ค้น user จาก DB ด้วย Prisma แล้วเทียบรหัสผ่านที่ hash ไว้
           // คืน object ของ user ถ้าถูกต้อง, คืน null ถ้าไม่ถูกต้อง
           return null
         },
       }),
     ],
   })
   ```
4. **สร้าง route handler ให้ Auth.js** ที่ `src/app/api/auth/[...nextauth]/route.ts`:
   ```ts
   import { handlers } from "@/auth"
   export const { GET, POST } = handlers
   ```
5. **(สำหรับ protected route) สร้างไฟล์ `proxy.ts`** ที่ root หรือใน `src/` (ดู §9.10):
   ```ts
   export { auth as middleware } from "@/auth"
   ```
6. **hash password** สำหรับ Credentials provider — ติดตั้งไลบรารี hash (เลือก `bcryptjs` ใช้ง่ายบน Node ไม่ต้อง build):
   ```bash
   npm install bcryptjs
   ```
   - ตอนสมัคร (signup): `hash` รหัสผ่านก่อนบันทึกลง DB.
   - ตอน login: ใช้ `compare` เทียบรหัสที่กรอกกับค่าที่ hash ไว้.
   - ⛔ **ห้ามเก็บรหัสผ่านเป็น plain text เด็ดขาด** (ย้ำคอนเซปต์ security ใน Phase 5).

> Phase 5 ใน CURRICULUM.md เป็น step-by-step ละเอียด (schema `User`, ฟอร์ม signup ที่ hash, กรอง task ตาม userId) — ส่วนนี้คือ "ติดตั้ง + โครงไฟล์" ให้พร้อมเท่านั้น.

---

## 13. Vitest (Phase 7 — unit/component test)

**ทำไม?** Phase 7 เขียน test เบื้องต้น. Vitest = test runner ที่เร็วและตั้งค่าง่าย เข้ากับโปรเจกต์ Vite/Next ได้ดี.

1. **ติดตั้ง Vitest + เครื่องมือทดสอบ component (React Testing Library) เป็น dev dependency:**
   ```bash
   npm install -D vitest @vitejs/plugin-react jsdom @testing-library/react @testing-library/jest-dom @testing-library/dom
   ```
2. **สร้างไฟล์ config `vitest.config.ts`** ที่ root โปรเจกต์:
   ```ts
   import { defineConfig } from "vitest/config"
   import react from "@vitejs/plugin-react"

   export default defineConfig({
     plugins: [react()],
     test: {
       environment: "jsdom",        // จำลอง DOM ของเบราว์เซอร์ให้ component test
       globals: true,                // ใช้ describe/it/expect ได้โดยไม่ต้อง import
     },
   })
   ```
3. **เพิ่ม script ลง `package.json`** ในส่วน `"scripts"`:
   ```json
   "test": "vitest"
   ```
4. **เขียน test แรก** เช่น `src/components/TaskItem.test.tsx` แล้วรัน:
   ```bash
   npm test
   ```
   - Vitest จะ watch ไฟล์และรันใหม่อัตโนมัติเมื่อแก้โค้ด. หยุดด้วย `Ctrl + C`.

> ⚠️ ถ้า component test ฟ้องหา matcher อย่าง `toBeInTheDocument` ให้สร้างไฟล์ setup ที่ `import "@testing-library/jest-dom"` แล้วชี้ `test.setupFiles` ใน config ไปที่ไฟล์นั้น (ผู้สอนดู docs Testing Library ตอนนั้น).

---

## 14. Playwright (Phase 7 — e2e test)

**ทำไม?** Playwright จำลองผู้ใช้กดจริงบนเบราว์เซอร์ (end-to-end) — Phase 7 ทำ flow สำคัญ 1 อัน เช่น "เพิ่ม task แล้วเห็นในรายการ".

1. **ติดตั้ง + ตั้งค่าด้วยตัวช่วย (init):**
   ```bash
   npm init playwright@latest
   ```
   ตอบ prompt (ภาษาอังกฤษ) ตามนี้:
   | คำถาม | กดตอบ |
   |---|---|
   | `Where to put your end-to-end tests?` | **e2e** (หรือ `tests` — เลี่ยงชื่อชนกับ unit test) |
   | `Add a GitHub Actions workflow?` | **No** (ยังไม่ทำ CI ในหลักสูตร) |
   | `Install Playwright browsers?` | **Yes** (ต้องมีเบราว์เซอร์ให้ Playwright ใช้) |
2. **(ครั้งแรกเท่านั้น) ถ้าเบราว์เซอร์ยังไม่ลง ให้สั่งลงเอง:**
   ```bash
   npx playwright install
   ```
3. **ให้ Playwright เปิด dev server ให้อัตโนมัติตอนรัน test** — ในไฟล์ `playwright.config.ts` ที่ init สร้างให้ ตั้ง `webServer`:
   ```ts
   webServer: {
     command: "npm run dev",
     url: "http://localhost:3000",
     reuseExistingServer: true,
   },
   ```
4. **รัน e2e test:**
   ```bash
   npx playwright test
   ```
   - ดูผลแบบมีรายงานหน้าเว็บ: `npx playwright show-report`.

> 💡 มือใหม่: ใช้ `npx playwright test --ui` เปิดโหมด UI เห็นการกดทีละขั้น เข้าใจง่ายกว่า.

---

## 14.5 หมายเหตุเรื่อง backup (push GitHub บ่อยๆ = backup) — อ่านก่อน Phase 8

**แนวคิด:** การ `git push` ขึ้น GitHub บ่อยๆ ถือเป็นการ **backup โค้ดไป cloud** ในตัว — ถ้าเครื่องพัง/หาย ก็ `git clone` กลับมาได้. ให้ผู้เรียนติดนิสัย commit + push เป็นระยะ (จบแต่ละขั้นย่อย/แต่ละเฟส).

> ⚠️ **แต่ของ 2 อย่างนี้ "ไม่ถูก backup" เพราะ gitignore ไว้** (create-next-app + Prisma ตั้งให้ตั้งแต่แรก):
> - **`.env`** — มีค่าลับเช่น `AUTH_SECRET` (§12) และ `DATABASE_URL`. ไม่ขึ้น git โดยตั้งใจ (กันความลับหลุด).
> - **`prisma/dev.db`** — ไฟล์ฐานข้อมูล SQLite (§11). ไม่ขึ้น git เช่นกัน.
>
> **ผลที่ตามมาเมื่อ clone ใหม่ / เครื่องพัง:** โค้ดกลับมาครบ แต่ `.env` กับ `dev.db` **จะหายไป** ทำให้แอป **รันไม่ได้ทันที**. ต้องทำ 2 ขั้นนี้เองหลัง clone:
> 1. **สร้าง `.env` ใหม่** — ใส่ `DATABASE_URL` กลับ แล้วรัน `npx auth secret` เพื่อ generate `AUTH_SECRET` ใหม่ (ดู §12). *(ค่า `AUTH_SECRET` ใหม่จะทำให้ session เดิมที่ login ไว้ใช้ไม่ได้ ต้อง login ใหม่ — ปกติ.)*
> 2. **สร้างฐานข้อมูลใหม่** — รัน `npx prisma migrate dev` เพื่อสร้างไฟล์ `dev.db` + ตารางขึ้นมาจาก migration history (ซึ่งอยู่ใน git อยู่แล้ว). ดู §11.
>
> 💡 สรุป: git/GitHub backup **โค้ด + migration history** ให้ แต่ **ไม่ backup ความลับและข้อมูลใน DB** — สองอย่างนั้นต้องสร้างใหม่เองทุกครั้งที่ตั้งเครื่องใหม่.

---

## 15. Postgres บน cloud — Neon หรือ Supabase (Phase 8 — Deploy)

**ทำไม?** ตอน deploy จริง SQLite (ไฟล์เดียวบนเครื่อง) ใช้ไม่ได้บน Vercel — ต้องย้ายไป **Postgres** ที่รันบน cloud. เลือก **Neon** หรือ **Supabase** อย่างใดอย่างหนึ่ง (มี free tier ทั้งคู่). แนะนำ **Neon** สำหรับมือใหม่ (ตั้งค่าน้อย ได้ connection string เร็ว).

### 15.1 สร้าง database บน Neon (แนะนำ)
1. ไปที่ <https://neon.tech> → สมัคร (Continue with GitHub ได้).
2. **Create project** → ตั้งชื่อ เช่น `taskflow` → เลือก region ใกล้ตัว → สร้าง.
3. หน้า dashboard จะมี **Connection string** (ขึ้นต้น `postgresql://...`) — กดคัดลอก (เลือกแบบ "Pooled connection" ได้สำหรับ serverless/Vercel).

### 15.2 (ทางเลือก) Supabase
1. ไปที่ <https://supabase.com> → สมัคร → **New project** (ตั้งรหัสผ่าน database เก็บไว้ดีๆ).
2. ไปที่ **Project Settings → Database → Connection string** เลือกแบบ **URI** แล้วคัดลอก (แทน `[YOUR-PASSWORD]` ด้วยรหัสที่ตั้งไว้).

### 15.3 ปรับ Prisma จาก SQLite → Postgres
1. ในไฟล์ `prisma/schema.prisma` เปลี่ยน provider ของ datasource:
   ```prisma
   datasource db {
     provider = "postgresql"   // เดิมเป็น "sqlite"
     url      = env("DATABASE_URL")
   }
   ```
2. ใส่ connection string ที่คัดลอกมาลง `.env` (เครื่อง local ใช้ทดสอบ):
   ```
   DATABASE_URL="postgresql://...ค่าที่คัดลอกมา..."
   ```
3. **รัน migration ขึ้น Postgres** (สร้างตารางบน DB cloud):
   ```bash
   npx prisma migrate deploy
   ```
   > ถ้าเริ่ม migration ใหม่บน DB เปล่าและต้องการสร้าง migration history ใหม่ ให้ใช้ `npx prisma migrate dev --name init_postgres` ในเครื่อง dev แทน. ผู้สอนเลือกตามสถานการณ์ (ดู docs Prisma เรื่อง deploy).
4. ทดสอบ local ที่ชี้ไป Postgres ว่ายังทำงาน แล้วค่อยไปขั้น deploy Vercel.

### 15.4 ตั้ง environment variables บน Vercel (ตอน deploy)
- บน Vercel → โปรเจกต์ → **Settings → Environment Variables** เพิ่ม **`DATABASE_URL`** (ค่า Postgres) และ **`AUTH_SECRET`** (ค่าเดียวกับที่สร้างใน §12 หรือสร้างใหม่สำหรับ production).
- ⛔ **อย่าใส่ค่าลับเหล่านี้ในโค้ดหรือ commit ขึ้น git** — ตั้งบน Vercel เท่านั้น.
- หลังตั้ง env แล้ว trigger deploy ใหม่ 1 ครั้งให้ค่ามีผล.

> Phase 8 ใน CURRICULUM.md จะพาทำ deploy เต็มขั้น (เชื่อม GitHub repo กับ Vercel, build, ทดสอบ production) — ส่วนนี้คือคำสั่ง/ค่าที่ต้องเตรียม.