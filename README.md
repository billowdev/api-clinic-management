# api-clinic-management
Example api on go in clinic management usecaase and ER Diagram


# Business Context


## Employee Management
```sql
Table Employee [NOTE: "ข้อมูลพนักงาน แสดงรายละเอียดของพนักงานในระบบ"] {
  id UUID [pk, unique, NOTE: "รหัสประจำตัวพนักงานที่ไม่ซ้ำกัน."]
  name VARCHAR(100) [NOTE: "ชื่อพนักงาน."]
  surname VARCHAR(100) [NOTE: "นามสกุลพนักงาน."]
  email VARCHAR(255) [NOTE: "อีเมลของพนักงาน."]
  username VARCHAR(100) [NOTE: "ชื่อผู้ใช้สำหรับเข้าระบบ."]
  password VARCHAR(255) [NOTE: "รหัสผ่านของพนักงาน."]
  sex VARCHAR(10) [NOTE: "เพศของพนักงาน."]
  phone VARCHAR(20) [NOTE: "เบอร์โทรศัพท์ของพนักงาน."]
  permission PermissionEnum [NOTE: "สิทธิการเข้าถึงของพนักงาน (เจ้าของ, พนักงาน)."]
  address VARCHAR(255) [NOTE: "ที่อยู่เลขที่/หมู่."]
  subdistrict VARCHAR(100) [NOTE: "ตำบลของพนักงาน."]
  district VARCHAR(100) [NOTE: "อำเภอของพนักงาน."]
  province VARCHAR(100) [NOTE: "จังหวัดของพนักงาน."]
  zipcode VARCHAR(10) [NOTE: "รหัสไปรษณีย์ของพนักงาน."]
  created_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was created."]
  updated_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was last updated."]
}
```

## Patient Management
```sql
Table Patient [NOTE: "ข้อมูลคนไข้ แสดงรายละเอียดของคนไข้ในระบบ"] {
  id UUID [pk, unique, NOTE: "รหัสประจำคนไข้ที่ไม่ซ้ำกัน."]
  name VARCHAR(100) [NOTE: "ชื่อคนไข้."]
  surname VARCHAR(100) [NOTE: "นามสกุลคนไข้."]
  id_card VARCHAR(20) [NOTE: "รหัสประจำตัวประชาชนของคนไข้."]
  sex VARCHAR(10) [NOTE: "เพศของคนไข้."]
  birth_date DATE [NOTE: "วันเกิดของคนไข้."]
  blood_type VARCHAR(10) [NOTE: "กรุ๊ปเลือดของคนไข้."]
  disease TEXT [NOTE: "โรคประจำตัวของคนไข้."]
  allergic TEXT [NOTE: "ประวัติการแพ้ของคนไข้."]
  phone VARCHAR(20) [NOTE: "เบอร์โทรศัพท์ของคนไข้."]
  address VARCHAR(255) [NOTE: "ที่อยู่เลขที่/หมู่."]
  subdistrict VARCHAR(100) [NOTE: "ตำบลของคนไข้."]
  district VARCHAR(100) [NOTE: "อำเภอของคนไข้."]
  province VARCHAR(100) [NOTE: "จังหวัดของคนไข้."]
  zipcode VARCHAR(10) [NOTE: "รหัสไปรษณีย์ของคนไข้."]
  created_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was created."]
  updated_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was last updated."]
}
```

## Appointment Management
```sql
Table Appointment [NOTE: "ข้อมูลการนัดหมาย แสดงรายละเอียดการนัดหมายระหว่างพนักงานกับคนไข้"] {
  id UUID [pk, unique, NOTE: "รหัสประจำตารางการนัดหมายที่ไม่ซ้ำกัน."]
  subject VARCHAR(255) [NOTE: "หัวข้อของการนัดหมาย."]
  detail TEXT [NOTE: "รายละเอียดของการนัดหมาย."]
  date TIMESTAMP [NOTE: "วันที่นัดหมาย."]
  status VARCHAR(100) [NOTE: "สถานะการนัดหมาย (เช่น 'Pending', 'Completed')."]
  patient_id UUID [ref: > Patient.id, NOTE: "อ้างอิงถึงข้อมูลคนไข้ที่นัดหมาย."]
  employee_id UUID [ref: > Employee.id, NOTE: "อ้างอิงถึงข้อมูลพนักงานที่จัดนัดหมาย."]
  created_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was created."]
  updated_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was last updated."]
}
```

## Material Management
```sql
Table Material [NOTE: "ข้อมูลวัสดุ แสดงรายละเอียดของวัสดุในระบบ"] {
  id UUID [pk, unique, NOTE: "รหัสประจำวัสดุที่ไม่ซ้ำกัน."]
  name VARCHAR(255) [NOTE: "ชื่อวัสดุ."]
  detail TEXT [NOTE: "รายละเอียดวัสดุ."]
  unit VARCHAR(50) [NOTE: "หน่วยนับของวัสดุ."]
  purchase_price DECIMAL(10, 2) [NOTE: "ราคาซื้อวัสดุ."]
  qty INT [NOTE: "จำนวนวัสดุในสต็อก."]
  agent_id UUID [ref: > Agent.id, NOTE: "อ้างอิงถึงข้อมูลตัวแทนจำหน่าย."]
  created_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was created."]
  updated_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was last updated."]
}

Table MaterialOrder [NOTE: "ข้อมูลการสั่งซื้อวัสดุ แสดงรายละเอียดการสั่งซื้อวัสดุ"] {
  id UUID [pk, unique, NOTE: "รหัสการสั่งซื้อวัสดุที่ไม่ซ้ำกัน."]
  total_price DECIMAL(10, 2) [NOTE: "ราคารวมของการสั่งซื้อ."]
  purchase_date DATE [NOTE: "วันที่ทำการสั่งซื้อ."]
  receive_date DATE [NOTE: "วันที่ได้รับวัสดุ."]
  status VARCHAR(100) [NOTE: "สถานะการสั่งซื้อ."]
  employee_id UUID [ref: > Employee.id, NOTE: "อ้างอิงถึงข้อมูลพนักงานที่ทำการสั่งซื้อ."]
  agent_id UUID [ref: > Agent.id, NOTE: "อ้างอิงถึงข้อมูลตัวแทนจำหน่าย."]
  created_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was created."]
  updated_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was last updated."]
}

Table MaterialOrderDetail [NOTE: "ข้อมูลรายละเอียดการสั่งซื้อวัสดุ แสดงรายละเอียดวัสดุที่สั่งซื้อในแต่ละรายการ"] {
  material_order_id UUID [ref: > MaterialOrder.id, NOTE: "อ้างอิงถึงข้อมูลการสั่งซื้อวัสดุ."]
  material_id UUID [ref: > Material.id, NOTE: "อ้างอิงถึงข้อมูลวัสดุ."]
  qty INT [NOTE: "จำนวนวัสดุที่สั่งซื้อ."]
  price DECIMAL(10, 2) [NOTE: "ราคาแต่ละหน่วยของวัสดุ."]
  created_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was created."]
  updated_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was last updated."]
}

Table MaterialDisburse [NOTE: "ตารางการเบิกจ่ายวัสดุ แสดงรายละเอียดการเบิกจ่ายวัสดุโดยพนักงาน"] {
  id UUID [pk, unique, NOTE: "รหัสตารางการเบิกจ่ายวัสดุที่ไม่ซ้ำกัน."]
  date TIMESTAMP [NOTE: "วันที่เบิกจ่ายวัสดุ."]
  employee_id UUID [ref: > Employee.id, NOTE: "อ้างอิงถึงข้อมูลพนักงานที่ทำการเบิกจ่าย."]
  created_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was created."]
  updated_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was last updated."]
}

Table MaterialDisburseDetail [NOTE: "รายละเอียดการเบิกจ่ายวัสดุ"] {
  qty INT [NOTE: "จำนวนวัสดุที่เบิกจ่าย."]
  material_id UUID [ref: > Material.id, NOTE: "อ้างอิงถึงข้อมูลวัสดุที่เบิกจ่าย."]
  employee_id UUID [ref: > Employee.id, NOTE: "อ้างอิงถึงข้อมูลพนักงานที่ทำการเบิกจ่าย."]
}
```

## Agent Management
```sql
Table Agent [NOTE: "ข้อมูลตัวแทนจำหน่าย แสดงรายละเอียดของตัวแทนจำหน่ายในระบบ"] {
  id UUID [pk, unique, NOTE: "รหัสประจำตัวตัวแทนจำหน่ายที่ไม่ซ้ำกัน."]
  name VARCHAR(255) [NOTE: "ชื่อตัวแทนจำหน่าย."]
  address VARCHAR(255) [NOTE: "ที่อยู่ของตัวแทนจำหน่าย."]
  phone VARCHAR(20) [NOTE: "เบอร์โทรศัพท์ของตัวแทนจำหน่าย."]
  email VARCHAR(255) [NOTE: "อีเมลของตัวแทนจำหน่าย."]
  created_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was created."]
  updated_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was last updated."]
}
```

## Medicine Management
```sql
Table Medicine [NOTE: "ข้อมูลยา แสดงรายละเอียดของยาที่มีในระบบ"] {
  id UUID [pk, unique, NOTE: "รหัสประจำยาที่ไม่ซ้ำกัน."]
  name VARCHAR(255) [NOTE: "ชื่อยา."]
  detail TEXT [NOTE: "รายละเอียดยา."]
  dosage VARCHAR(100) [NOTE: "ขนาดยา."]
  created_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was created."]
  updated_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was last updated."]
}
```

## Order Management
```sql
Table Order [NOTE: "ข้อมูลการสั่งซื้อ แสดงรายละเอียดการสั่งซื้อยาและวัสดุ"] {
  id UUID [pk, unique, NOTE: "รหัสการสั่งซื้อที่ไม่ซ้ำกัน."]
  patient_id UUID [ref: > Patient.id, NOTE: "อ้างอิงถึงข้อมูลคนไข้ที่ทำการสั่งซื้อ."]
  total_price DECIMAL(10, 2) [NOTE: "ราคารวมของการสั่งซื้อ."]
  status VARCHAR(100) [NOTE: "สถานะการสั่งซื้อ (เช่น 'Pending', 'Completed')."]
  created_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was created."]
  updated_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was last updated."]
}

Table OrderDetail [NOTE: "รายละเอียดการสั่งซื้อ แสดงรายละเอียดการสั่งซื้อแต่ละรายการ"] {
  order_id UUID [ref: > Order.id, NOTE: "อ้างอิงถึงข้อมูลการสั่งซื้อ."]
  medicine_id UUID [ref: > Medicine.id, NOTE: "อ้างอิงถึงข้อมูลยา."]
  qty INT [NOTE: "จำนวนยาที่สั่งซื้อ."]
  price DECIMAL(10, 2) [NOTE: "ราคาแต่ละหน่วยของยา."]
  created_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was created."]
  updated_at TIMESTAMP [default: `CURRENT_TIMESTAMP`, NOTE: "Timestamp when the record was last updated."]
}

Table Course [NOTE: "ข้อมูลคอร์สหรือการรักษา"] {
  id UUID [pk, unique, NOTE: "รหัสประจำข้อมูลการรักษา."]
  date TIMESTAMP [NOTE: "วันที่รับการรักษา."]
  weight DECIMAL [NOTE: "น้ำหนักของคนไข้."]
  height DECIMAL [NOTE: "ส่วนสูงของคนไข้."]
  systolic INT [NOTE: "ความดันช่วงบน."]
  diastolic INT [NOTE: "ความดันช่วงล่าง."]
  heart_rate INT [NOTE: "อัตราการเต้นของหัวใจ."]
  diagnose TEXT [NOTE: "การวินิจฉัย."]
  total_price DECIMAL [NOTE: "ราคารักษา."]
  status VARCHAR [NOTE: "สถานะการรักษา."]
  patient_id UUID [ref: > Patient.id, NOTE: "อ้างอิงถึงข้อมูลคนไข้."]
  employee_id UUID [ref: > Employee.id, NOTE: "อ้างอิงถึงข้อมูลพนักงาน."]
}


Table Receipt [NOTE: "ใบเสร็จ"] {
  id UUID [pk, unique, NOTE: "รหัสใบเสร็จ."]
  date TIMESTAMP [NOTE: "วันที่ใบเสร็จ."]
  total_price DECIMAL [NOTE: "ราคารวมทั้งหมด ทั้งค่ารักษาและยา."]
  course_id UUID [ref: > Course.id, NOTE: "อ้างอิงถึงข้อมูลการรักษา."]
  medicine_disburse_id UUID [ref: > MedicineDisburse.id, NOTE: "อ้างอิงถึงข้อมูลการจ่ายยา."]
  patient_id UUID [ref: > Patient.id, NOTE: "อ้างอิงถึงข้อมูลคนไข้."]
  employee_id UUID [ref: > Employee.id, NOTE: "อ้างอิงถึงข้อมูลพนักงาน."]
}
```