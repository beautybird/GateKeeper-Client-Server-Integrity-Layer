
![gatekeeper_logo](https://github.com/user-attachments/assets/2d204146-e5e0-4304-ba9e-7531a914e850)




# GateKeeper-Client-Server-Integrity-Layer

A security layer sits in front of the connection between a server connection with the client

GateKeeper – Client ↔ Server Integrity Layer 

  Stop replay, spoofing & abuse in 10 minutes.
  
  GateKeeper drops a hardened Edge Function in front of your backend and gives your Flutter & iOS
apps a cryptographically signed "handshake" on every request.

GateKeeper Basic : Deal with following threats. 



 ✨ Features

| Layer         | What EdgeSecure adds                               |
| ------------- | -------------------------------------------------- |
| Transport | Enforces HTTPS, HSTS, no‑compression bodies        |
| Identity  | Verifies Supabase JWT on each call                 |
| Freshness | ±30 s timestamp check to block replay              |
| Integrity | HMAC‑SHA‑256 signature |
| Abuse     | Per‑user rate‑limit         |
| Logging   | Safe insert into `public.messages` (optional)      |
| Clock‑skew| manipulation  |
| Reject oversized | Reject oversized |
| JSON‑schema validation | Strict JSON‑schema validation (Zod) |



Every 15 minutes the checks automatically will run against threats .

## Invocation 

![edge_function_invocations](https://github.com/user-attachments/assets/490abb6c-d6f3-47bb-8c31-37e89d7037bc)


## Logs the result of the checks


![Edge_function_log](https://github.com/user-attachments/assets/32bc8a4d-b066-4699-a111-b98ed2665a9d)



GateKeeper Advanced  : Deal with basic threats +  other newer sophisticated attacks  .


   🚀 Quick‑start (10 min)

NOTES: 

For tables, policies,triggers & functions and the edge function ,You may run all the code at the Supabase SQL editor (preffered) or bash CLI .

Current codes apply for Supabase Platform. For dedicated Postgresql comming soon.

### 1 · Clone & run the setup SQL

    supabase link --project-ref <your‑project‑ref>
    psql -f supabase/sql/create_messages.sql
    psql -f supabase/sql/create_rate_limits.sql
    psql -f supabase/sql/init_edge_secret_trigger.sql

(use the SQL editor if you prefer copy‑paste)

### 2 · Deploy the Edge Function

    cd supabase/functions/edge_secure_app_connection
    supabase functions deploy edge_secure_app_connection \
      --env SUPABASE_URL=https://<project>.supabase.co \
      --env SUPABASE_ANON_KEY=<anon‑key>

### 3 · Add the client helper

#### Flutter

    import 'secure_handshake.dart';
    
    Future<void> main() async {
      await Supabase.initialize(...);
      setupSecureHandshakeListener();   // one‑liner 🔒
      runApp(MyApp());
    }

#### Swift

    let supabase = SupabaseClient(url: "…", key: "…")
    let handshake = SecureHandshake(supabase: supabase) // auto‑listener 🔒

### 4 · Send a secured message (example)

    await supabase.functions.invoke(
      'edge_secure_app_connection',
      body: { 'message': 'Hello world' },
    );



 📁 Repo structure

```
edge-secure-connection/
├── supabase/
│   ├── functions/edge_secure_app_connection/index.ts
│   └── sql/ (schema + helper functions)
├── client/
│   ├── flutter/secure_handshake.dart
│   └── ios/SecureHandshake.swift
├── example_flutter/ (optional demo)
└── README.md (this file)



 🛡️ Security flow (server‑side)


1  HTTPS enforced (x‑forwarded-proto=https)
2  Validate JWT via supabase.auth.getUser()
3  Timestamp ±30 s
4  HMAC(body+timestamp) w/ per‑user edge_secret
5  bump_rate_limit(p_uid, p_limit)  (atomic)
6  Insert message   (optional business logic)


If any step fails the function returns **401 / 413 / 429** and no DB write is
performed.


 📚 Documentation

* /supabase/sql/** – self‑contained schema & RLS scripts
* /client/** – drop‑in helpers
* /docs/** (planned) – Threat model, FAQ, troubleshooting


 💼 License & commercial use


* Commercial** licence with email support & SLAs available – contact
  `deziretechcom@gmail.com`.



 Need help?
File an issue or email us – we’ll respond within 24 h.
