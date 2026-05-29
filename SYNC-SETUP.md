# Cross-device sync setup (Supabase)

The "attending" marks sync across devices via a Supabase table. Until the keys
are filled in, the radar runs in **local-only mode** (this browser only) — it
still works, marks just don't sync.

## One-time setup

1. Create a free project at https://supabase.com (note the **Project URL** and
   the **anon public** API key under *Project Settings → API*).

2. In the Supabase **SQL Editor**, run:

   ```sql
   create table public.attending (
     id         text primary key,
     marked     boolean not null default true,
     updated_at timestamptz not null default now()
   );

   alter table public.attending enable row level security;

   -- Open editing: anyone on the public page can read/mark/unmark.
   create policy "open read"   on public.attending for select using (true);
   create policy "open insert" on public.attending for insert with check (true);
   create policy "open update" on public.attending for update using (true) with check (true);
   ```

3. Send me the **Project URL** + **anon public key** (or paste them yourself
   into the `SUPABASE_URL` / `SUPABASE_ANON_KEY` constants at the top of the
   `<script>` block in `index.html`).

That's it — once both values are set, marking an event on any device writes to
Supabase, and every device pulls on load, on tab focus, and every 60 seconds.

## How it works

- `marked = true` rows are the attending set; unmarking upserts `marked = false`.
- `localStorage` is kept as an instant-UI cache and offline fallback; the
  Supabase table is the source of truth when sync is enabled.
- The anon key is public by design (the page is public, open-editing). Anyone
  visiting the live page can toggle marks — switch the RLS policies to require
  auth later if that changes.
