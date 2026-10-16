# Final Exam – Software Testing with Playwright (SauceDemo)

ระบบเป้าหมาย: https://www.saucedemo.com/ | เครื่องมือ: Playwright Test (TypeScript)

## วิธีรัน
```
npm install
npx playwright test
npx playwright show-report
```

## โครงสร้าง
- `tests/unit/` UT-01, UT-02 (UI Component / Functional Testing แบบ Black-box)
- `tests/integration/` IT-01 (Top-Down + Stub), IT-02 (Bottom-Up + Driver)
- `tests/system/` ST-01, ST-02 (System Testing)
- `playwright-report/` HTML Report, `reports/results.json` JSON Report

## IT-01: Top-Down Integration Testing ด้วย Stub
- **Real:** หน้า Login ของ SauceDemo, การกรอก standard_user / secret_sauce, การนำทางไปยัง `inventory.html`
- **Stub:** หน้า Inventory ที่สร้างด้วย `page.setContent()` แสดง `Mock Inventory Ready`, `Mock Backpack`, `Mock Bike Light`
- Stub นี้จำลอง DOM ระดับ Browser หลัง Login จริง ไม่ได้แทน Internal Module ของ SauceDemo
- แนวคิด Top-Down: ทดสอบโมดูลระดับบน (Login) ของจริงก่อน แล้วใช้ Stub แทนโมดูลระดับล่างที่ยังไม่พร้อม/ไม่ต้องการใช้ของจริง (Inventory)

## IT-02: Bottom-Up Integration Testing ด้วย Driver
- **Driver:** ฟังก์ชัน `driverOpenInventory()` ตั้ง Cookie `session-username=standard_user` ด้วย `BrowserContext.addCookies()` แล้วเปิด `inventory.html` โดยตรง ทำหน้าที่แทนโมดูลระดับบน (Login) เพื่อเรียกใช้โมดูลระดับล่าง
- **Real:** หน้า Inventory (6 สินค้า), การเพิ่มสินค้า, Cart Badge และหน้า Shopping Cart ของ SauceDemo จริง
- แนวคิด Bottom-Up: ทดสอบโมดูลระดับล่างของจริงก่อน โดยใช้ Driver เรียกใช้งานแทนโมดูลระดับบนที่ยังไม่ถูกนำมาเชื่อม
