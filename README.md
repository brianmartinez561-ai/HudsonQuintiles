# Quintile Sales Dashboard (Netlify version)

A store quintile dashboard with shared saving and passwords. It runs on your own
Netlify site and does not need claude.ai.

- **Everyone sees the same data.** Data is saved on Netlify (in Netlify Blobs), so any
  phone or computer that opens the site sees the latest loaded months.
- **Two passwords:**
  - `EDIT_PASSWORD` (required): for managers who load Excel data and change weights or cut-offs.
  - `VIEW_PASSWORD` (optional): if you set it, people need it just to look. If you
    leave it unset, anyone with the link can view but only editors can change anything.

## What's in the folder

```
public/index.html              the whole dashboard (HTML, CSS, JavaScript)
netlify/functions/data.mjs     /api/data: checks passwords, reads and saves the data
netlify.toml                   tells Netlify where things are, plus security headers
package.json                   the one dependency (@netlify/blobs)
```

## Deploy (pick one)

> Netlify Drop (dragging the folder onto app.netlify.com/drop) does not install
> `@netlify/blobs`, so saving will not work that way. Use option A or B.

**A. From GitHub (easiest to update later)**
1. Unzip, and push the `netlify-sales-dashboard` folder to a new GitHub repository.
2. In Netlify: **Add new project > Import an existing project**, then pick the repo.
   The settings come from `netlify.toml`, so leave the build command empty.
3. Set the passwords (see below), then **Deploys > Trigger deploy**.

**B. With the Netlify CLI (from your computer)**
```bash
cd netlify-sales-dashboard
npm install
npx netlify-cli login
npx netlify-cli deploy --prod        # creates or links a site the first time
```
Set the passwords (see below), then run `npx netlify-cli deploy --prod` again.

## Set the passwords

Netlify > your site > **Project configuration > Environment variables > Add a variable**:

| Key             | Value                 | Needed?  |
|-----------------|-----------------------|----------|
| `EDIT_PASSWORD` | a long, unique phrase | Required |
| `VIEW_PASSWORD` | another phrase        | Optional |

Make sure the variables are available to **Functions**. Redeploy after you add or change
them. To lock someone out, change the password and redeploy.

## Using it

1. Open the site and sign in with the edit password. Ticking "Remember me" keeps you
   signed in on that device.
2. Go to **Load data**. Drop in the monthly Excel export, or copy the table in Excel and
   paste it. The fictional demo stores are removed automatically the first time you load
   real data.
3. Share the site link and the view password (if you set one) with store teams. They can
   use the Overview, My store, the planner and what-if, but they can't change the data.

## Good to know

- Opening `public/index.html` straight from your computer works too, but with no server
  it saves on that device only. The status in the top bar says "Offline".
- If two editors save at the same time, the second one is told and gets the newer data,
  so nobody overwrites anyone without knowing.
- Wrong passwords are slowed down on purpose to make guessing harder. Still use long passwords.
- **Load data > Download backup** saves a copy of everything as a .json file. Paste a backup
  into the box there to restore it.
- The Excel reader (SheetJS) and fonts load from public CDNs. If your network blocks
  them, use "Paste from Excel" instead of uploading.
