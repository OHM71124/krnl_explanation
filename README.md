## ทำไมถึงเปิดโปรแกรม krnl หลายจอไม่ได้?

![alt text](https://raw.githubusercontent.com/itsmeny/krnl_explanation/master/img/1.png)  

จาก code ด้านบนจะเห็นว่ามีการใช้ Process.GetProcess() และ for loop ในการเช็คหา process ที่มีชื่อว่า
* krnls
* krnlss 

ถ้าเจอ process ที่มีชื่อตรงกับชื่อเหล่านี้ใน list มันจะทำการปิดตัวเองทันที นั่นทำให้มันเปิดได้แค่โปรแกรมเดียว (1 instance)

> เปิดหลายโปรแกรมง่ายๆ ก็แค่เปลี่ยนชื่อโปรแกรมจาก krnls , krnlss เป็นอย่างอื่น LOL??

## หลังจาก inject  krnl.dll เข้าสู่เกมแล้ว ระบบการทำงานคร่าวๆมันเป็นยังไง?

![alt text](https://raw.githubusercontent.com/itsmeny/krnl_explanation/master/img/5.png)  

หลังจากที่กดปุ่ม INJECT ไปแล้วตัวโปรแกรม krnl จะ inject dll (krnl.dll) เข้าสู่ตัวเกมจาก ProcessName จาก code ด้านบน  
แต่ยังไงถ้าใช้แบบ Original แบบนี้ มันจะ inject แค่หน้าต่างเกม Roblox ที่เปิดล่าสุดเท่านั้น กรณีที่ GetProcess จาก ProcessName เพราะมันจะเอาแค่ค่า ProcessName แล้วแปลงเป็น ProcessId จากหน้าต่างเกมล่าสุดที่เปิด

> inject หลายจอง่ายๆ โดยแค่แก้ code นิดหน่อยจากที่มัน inject จากการ GetProcessByName แล้วแปลงเป็น ProcessId ก็ให้มัน inject เข้า ProcessId โดยตรงได้เลย!

![alt text](https://raw.githubusercontent.com/itsmeny/krnl_explanation/master/img/2.png)  

แล้ว krnl.dll ที่เรา Inject เข้าไปก็จะขึ้นหน้าต่าง cmd ที่ให้เราใส่ key นั่นแหละ มันจะสร้าง krnlpipe ขึ้นมา แล้วตัวโปรแกรม krnl ที่เป็นหน้าต่างก็จะ หา krnlpipe จาก code ด้านบน


## เปลี่ยนชื่อ แก้โค๊ด inject dll หลายจอแล้วทำไมยังใช้สคริปหลายจอไม่ได้?

ในส่วนนี้มันอยู่ที่ dll ของ krnl แล้วครับ (krnl.dll) รายละเอียดของ krnl.dll  
ทุกครั้งที่เรา Inject dll เข้าเกมไป มันจะสร้าง pipe ที่ชื่อว่า krnlpipe แล้วตัวโปรแกรมมันจะหาแค่ชื่อ pipe นี้เท่านั้น จาก code ด้านบน ที่ findpipe  
แถม krnl.dll ก็ถูก pack ไว้ด้วย vmprotect  

![alt text](https://raw.githubusercontent.com/itsmeny/krnl_explanation/master/img/3.png)  

ซึ่งนั่นมันทำให้ยากต่อการ แกะ , debug ตัว dll เพื่อแก้ไขชื่อของ krnlpipe
แต่ไม่เป็นไร ถึงมันจะ pack dll ไว้ก็เถอะ ถ้าเข้าไปดู string ในตัวเกม Roblox มันก็จะสามารถมองเห็นชื่อ pipe ได้จาก Memory ของเกม


![alt text](https://raw.githubusercontent.com/itsmeny/krnl_explanation/master/img/4.png)  

ต่อให้ inject dll หลายจอ เปิดโปรแกรม krnl หลายหน้าต่าง แต่ execute script แล้วยังไงมันก็เข้าไปที่จอเกมแค่จอเดียวอยู่ดี เพราะมันติดที่ krnlpipe

## ไอเดียของผมในการทำหลายจอ krnl

1. แก้วิธี Inject ให้เป็น Inject ไปที่ ProcessId เลย
2. แก้ krnlpipe ใน krnl.dll ให้เป็นชื่ออื่นๆ (ตัวอย่างเช่น krnlpipe , krnlpipe1 , krnlpipe2)
3. แก้ findpipe ให้เป็นชื่ออื่นๆที่ไม่ซ้ำกัน (ตัวอย่างเช่น ให้หาชื่อ pipe krnlpipe , krnlpipe1 , krnlpipe2)
4. enjoy :)

## note

หวังว่าบทความนี้จะมีประโยชน์ไม่มากก็น้อยสำหรับพวกคุณนะครับ :)

