# n8n Workflow: Sql Kpi

## Amaç

Bu workflow, PostgreSQL üzerinde temel veritabanı işlemlerini örneklemek için yapıldı

- Tablolar oluşturma
- JOIN ve UNION sorguları çalıştırma
- TRUNCATE ile tabloyu temizleme
- ALTER ile tabloya yeni kolon ekleyip veri ekleme

Workflow, tek bir Manual Trigger ile başlatılır.

---

## Kullanılan Servis ve Credentials

- **PostgreSQL container:** `Postgres kpi Acc` credential ile bağlı
- **Kullanıcı ve şifre:** Workflow credential’larında tanımlı
- **Database:** `kpi` (veya workflow içinde kullanılan tablolar)

---

## Adım Adım İşleyiş

1. **Manual Trigger**
   - Workflow’u başlatır. “Execute workflow” butonuna basıldığında çalışır.

2. **Tablo Oluşturma**
   - `Customers` ve `Orders` tabloları oluşturulur, örnek veriler eklenir:
     - Customers: Ali, Ayşe, Mehmet
     - Orders: Laptop, Telefon, Tablet

3. **ALTER & TRUNCATE Testi için Tablo Oluşturma**
   - `CustomersDel` ve `OrdersDel` tabloları oluşturulur.

4. **INNER JOIN**
   - `Customers` ve `Orders` tabloları `customer_id` üzerinden birleştirilir.
   - Amaç: JOIN kullanımını test etmek.

5. **UNION / UNION ALL**
   - `Customers.city` ve `Orders.product` sütunları birleştirilir.
     - `UNION`: Tekrarlı kayıtları çıkarır.
     - `UNION ALL`: Tekrarlı kayıtları gösterir.

6. **TRUNCATE TABLE**
   - `CustomersDel` tablosu temizlenir ve ID’ler sıfırlanır (`RESTART IDENTITY`).

7. **TRUNCATE Sonrası Kontrol**
   - `CustomersDel` tablosu kontrol edilir.

8. **ALTER ile Kolon Ekleme**
   - `CustomersDel` tablosuna yeni bir `email` sütunu eklenir.

9. **Veri Ekleme**
   - ALTER sonrası yeni sütunu doldurarak veri eklenir:
     - Ali → ali@example.com
     - Ayşe → ayse@example.com
     - Mehmet → mehmet@example.com

10. **Son Kontrol**
    - `CustomersDel` tablosundaki tüm veriler gösterilir.

---

Her adım, n8n üzerinde ayrı bir node olarak tanımlanmıştır. Workflow’u çalıştırmak için sadece Manual Trigger’a tıklamanız yeterlidir.