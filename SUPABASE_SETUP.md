# Supabase Setup za Handmade by DARA

## 1. Kreiraj tabelu `orders`

Idi u Supabase → SQL Editor i pokreni ovaj SQL:

```sql
create table if not exists public.orders (
  id bigserial primary key,
  name text not null,
  email text not null,
  phone text not null,
  address text not null,
  country text not null,
  note text,
  items text not null,
  total numeric not null,
  status text default 'Nova',
  created_at timestamp default now()
);

-- Enable RLS
alter table public.orders enable row level security;

-- Allow anyone to insert
create policy "Allow inserts" on public.orders
  for insert with check (true);

-- Allow admin to select all
create policy "Allow selects" on public.orders
  for select using (true);
```

## 2. Postavi email notifikacije (Opciono)

Za automatske email notifikacije kada nova narudžba stigne:

### Opcija A: Koristi Resend (preporučeno)
1. Kreiraj račun na https://resend.com (besplatno)
2. Uzmi API ključ
3. U Supabase, kreiraj Edge Function ili koristi Zapier/Make integraciju

### Opcija B: Koristi Gmail SMTP
1. Kreiraj App Password u Google Account Settings
2. Koristi u Edge Function

### Opcija C: Manual - Redovno provjeri Supabase dashboard
- Login u https://zkrifetqrtmmyvsfphyk.supabase.co
- Idi na "Database" → "orders" tabelu
- Vidjet ćeš sve nove narudžbe

## 3. Kako testirati

1. Idi na https://egonjicstanislav-afk.github.io/HandmadebyDara/
2. Dodaj torbe u korpu
3. Klikni "Nastavi na narudžbu"
4. Popuni formu i pošalji
5. Narudžba će biti sprema u `orders` tabeli

## Struktura podataka

```
orders table:
- id (auto)
- name (ime kupca)
- email (email kupca)
- phone (telefon)
- address (adresa dostave)
- country (država - BiH ili Srbija)
- note (napomena)
- items (stavke: "Torba 1 ×1, Torba 2 ×2")
- total (ukupna cijena)
- status (Nova, Obrađena, Poslana, itd)
- created_at (datum i vrijeme)
```

## Admin panel

U admin panelu možeš vidjeti sve narudžbe iz localStorage (trenutno).
Za Supabase integraciju u admin panelu, trebam dodatnu kodu.
