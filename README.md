┌─────────────────┐
│      USER       │
├─────────────────┤
│ PK user_id      │
│    email        │
│    password_hash│
│    role         │──────┐
│    created_at   │      │
│    updated_at   │      │
│    is_verified  │      │
│    is_active    │      │
└─────────────────┘      │
         │               │
         │ 1             │
         │               │
         │ *             │
    ┌────┴─────┬─────────┴──────┬─────────────┬──────────────┐
    │          │                │             │              │
┌───▼──────┐ ┌─▼──────────┐ ┌──▼──────────┐ ┌▼──────────┐  │
│  DONOR   │ │MEDICAL_STAFF│ │ CAMP_HOST   │ │   ADMIN   │  │
├──────────┤ ├─────────────┤ ├─────────────┤ ├───────────┤  │
│PK donor_id│ │PK staff_id  │ │PK host_id   │ │PK admin_id│  │
│FK user_id │ │FK user_id   │ │FK user_id   │ │FK user_id │  │
│  name     │ │  name       │ │  org_name   │ │  name     │  │
│  phone    │ │  phone      │ │  phone      │ │  phone    │  │
│  dob      │ │  position   │ │  address    │ │  dept     │  │
│  gender   │ │FK hospital_id│ │  city      │ └───────────┘  │
│  blood_grp│ │  license_no │ │  region     │                │
│  address  │ │  verified_at│ │  verified_at│                │
│  city     │ └─────────────┘ └─────────────┘                │
│  latitude │                                                 │
│  longitude│                                                 │
│  last_don │                                                 │
│  eligible │                                                 │
│  pref_not │                                                 │
└───────────┘                                                 │
     │                                                        │
     │ 1                                                      │
     │                                                        │
     │ *                                                      │
┌────▼─────────────┐                                         │
│ ELIGIBILITY_QUIZ │                                         │
├──────────────────┤                                         │
│PK quiz_id        │                                         │
│FK donor_id       │                                         │
│   responses      │ (JSON)                                  │
│   result         │                                         │
│   completed_at   │                                         │
└──────────────────┘                                         │
                                                              │
┌─────────────────┐         ┌──────────────────┐            │
│    HOSPITAL     │         │  DONATION_CAMP   │            │
├─────────────────┤         ├──────────────────┤            │
│PK hospital_id   │         │PK camp_id        │            │
│   name          │         │FK host_id        │────────────┘
│   address       │         │   name           │
│   city          │         │   address        │
│   phone         │         │   city           │
│   email         │         │   latitude       │
│   latitude      │         │   longitude      │
│   longitude     │         │   start_date     │
│   approved_by   │         │   end_date       │
│   approved_at   │         │   status         │
│   is_active     │         │   max_donors     │
└─────────────────┘         │   approved_by    │
     │                      │   approved_at    │
     │ 1                    └──────────────────┘
     │                              │
     │ *                            │ 1
┌────▼──────────────┐               │
│BLOOD_INVENTORY    │               │ *
├───────────────────┤      ┌────────▼──────────┐
│PK inventory_id    │      │ CAMP_VOLUNTEERS   │
│FK hospital_id     │      ├───────────────────┤
│   blood_group     │      │PK volunteer_id    │
│   component_type  │      │FK camp_id         │
│   quantity        │      │FK donor_id        │
│   bag_number      │      │   role            │
│   donated_date    │      │   registered_at   │
│   expiry_date     │      └───────────────────┘
│   status          │
│   location_rack   │
└───────────────────┘
     │
     │ *
     │
     │ 1
┌────▼────────────────┐
│ DONATION_RECORD     │
├─────────────────────┤
│PK donation_id       │
│FK donor_id          │
│FK staff_id          │
│FK hospital_id       │
│FK camp_id (nullable)│
│FK appointment_id    │
│   donation_date     │
│   blood_group       │
│   hemoglobin        │
│   blood_pressure    │
│   weight            │
│   bag_number        │
│   volume_ml         │
│   component_type    │
│   status            │
│   remarks           │
└─────────────────────┘
     │
     │ 1
     │
     │ 1
┌────▼─────────────────┐
│ CERTIFICATE          │
├──────────────────────┤
│PK certificate_id     │
│FK donation_id        │
│   cert_number        │
│   issued_date        │
│   qr_code_data       │
│   download_url       │
└──────────────────────┘

┌─────────────────────┐
│   APPOINTMENT       │
├─────────────────────┤
│PK appointment_id    │
│FK donor_id          │
│FK hospital_id       │
│FK camp_id (nullable)│
│   appointment_date  │
│   appointment_time  │
│   status            │
│   booking_type      │
│   qr_code           │
│   reminder_sent     │
│   created_at        │
│   cancelled_at      │
│   cancel_reason     │
└─────────────────────┘

┌─────────────────────┐
│  BLOOD_REQUEST      │
├─────────────────────┤
│PK request_id        │
│FK hospital_id       │
│FK requested_by      ���
│   blood_group       │
│   component_type    │
│   quantity_units    │
│   urgency_level     │
│   patient_name      │
│   required_by       │
│   status            │
│   fulfilled_at      │
│   created_at        │
└─────────────────────┘

┌─────────────────────┐
│  STOCK_TRANSFER     │
├─────────────────────┤
│PK transfer_id       │
│FK from_hospital_id  │
│FK to_hospital_id    │
│FK request_id        │
│FK approved_by       │
│   blood_group       │
│   component_type    │
│   quantity          │
│   bag_numbers       │ (JSON array)
│   transfer_date     │
│   status            │
│   remarks           │
└─────────────────────┘

┌─────────────────────┐
│   NOTIFICATION      │
├─────────────────────┤
│PK notification_id   │
│FK user_id           │
│   type              │
│   title             │
│   message           │
│   data              │ (JSON)
│   channel           │ (SMS/Email/Push)
│   status            │
│   sent_at           │
│   read_at           │
└─────────────────────┘

┌─────────────────────┐
│ EMERGENCY_MATCH     │
├─────────────────────┤
│PK match_id          │
│FK request_id        │
│FK donor_id          │
│   distance_km       │
│   match_score       │
│   notified_at       │
│   response          │
│   responded_at      │
└─────────────────────┘
