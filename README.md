# [ksana.in](https://ksana.in)

<img alt="ksana.in" src="public/images/orange/ksana.png" height="80"/>

✂️ Open source URL shortener, free to use and without ads

[![Deployment](https://img.shields.io/github/deployments/mazipan/ksana.in/production?label=vercel&logo=vercel&logoColor=white)](https://github.com/mazipan/ksana.in/deployments/activity_log?environment=Production) ![Website Up](https://img.shields.io/website-up-down-brightgreen-red/https/ksana.in.svg) ![](https://img.shields.io/endpoint?url=https%3A%2F%2Fksana.in%2Fapi%2Fshield)

## Features

- Fully open source
- Mobile First UI (best view in your mobile device)
- Full authentication flow
  - Login (with Email or third parties: Google, GitHub and Twitter)
  - Register manual using email
  - Forget Password and set new password
- Simple hit stats on every short URL
- Share the link using Native Share API for mobile device and fallback to WA, Twitter and Facebook share.
- Copy the link using new Clipboard API, fallback to old school copy-to-clipboard mechanism

## Screenshots

<table>
 <tbody>
   <tr>
     <td>
       <img alt="Homepage" src="screenshots/mobile-home.png" />
     </td>
     <td>
       <img alt="Homepage" src="screenshots/mobile-login.png" />
     </td>
     <td>
       <img alt="Homepage" src="screenshots/mobile-dashboard.png" />
     </td>
   </tr>
 </tbody>
</table>

## Installation

Copy file `.env.local.example` to `.env.local` and change value with your own supabase url and anonymous key.
You can get it after register and create your own project on Supabase.io.

```
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
```

## Creating table on Supabase

Go to the SQL tab and execute this query on the editor.

```sql
create table urls (
  id bigint generated by default as identity primary key,
  user_id uuid references auth.users not null,
  real_url text check (char_length(real_url) > 1),
  slug text check (char_length(slug) > 1),
  hit integer default 0,
  is_dynamic smallint default 0,
  inserted_at timestamp with time zone default timezone('utc'::text, now()) not null,
  updated_at timestamp with time zone default timezone('utc'::text, now()) not null
);
```

Click **RUN** to execute the query

## Creating triggers on Supabase

Go to the SQL tab and execute this query on the editor.

Add triggers for _"updated_at"_ field, copy this code on the same SQL editor

```sql
create extension if not exists moddatetime schema extensions;

-- this trigger will set the "updated_at" column to the current timestamp for every update
create trigger handle_updated_at before update on public.urls
  for each row execute procedure moddatetime (updated_at);
```

Click **RUN** to execute the query

## Creating View on Supabase

Go to the SQL tab and execute this query on the editor.

```sql
create view distinct_users as
    select distinct(user_id) from public.urls;
```

Click **RUN** to execute the query

## Additional settings for Authentication

- on Authentication setting, change `Site URL` to `/callback`. e.g: `https://ksana.in/callback`, for development just set it to `http://localhost:3000/callback`
- To support Google Login, in Authentication setting page, set the `Google Client ID` and `Google Secret`
- Increase JWT expiry time to `604800` for longer session. It's on on Authentication setting menu

## Add environment variables on Vercel

You can found all required environment variables on `.env.local.example`

## Register you own Splitbee

Ksana.in using Splitbee for Analytic, if you plan to deploy it by yourself, you might need to register your own Splitbee account.

## Can I deploy to my own domain?

The code is open for learning purpose.
But in case you didn't like the default domain (`ksana.in`), feel free to deploy to your own domain.
Since Ksana.in is using a free plan from Supabase, it have many limitation in term of size.
If you plan to use it in the bigger frequency, I suggest to deploy it with your own Supabase plan.

## The Apps based on Ksana.in's codebase

- https://hamsh.me/

## Credits

- [Next.js](https://nextjs.org/)
- [Supabase](https://supabase.io/)
- [Splitbee](https://splitbee.io/)
- [Chakra-UI](https://chakra-ui.com/docs/getting-started)
- [SWR](https://swr.vercel.app/)
- [React-Icons](https://react-icons.github.io/react-icons/)
- [Oge](https://oge.vercel.app/)
- Illustrations by [manypixels.co](https://www.manypixels.co/gallery)

## Support me

- 👉 🇮🇩 [Trakteer](https://trakteer.id/mazipan/tip?utm_source=github)
- 👉 🌍 [BuyMeACoffe](https://www.buymeacoffee.com/mazipan?utm_source=github)
- 👉 🌍 [Paypal](https://www.paypal.me/mazipan?utm_source=github)
- 👉 🌍 [Ko-Fi](https://ko-fi.com/mazipan)

---

Copyright ©️ 2021 by Irfan Maulana
