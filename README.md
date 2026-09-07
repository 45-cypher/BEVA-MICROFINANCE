# BEVA Microfinance Production v1.0.7 (imerekebishwa)

Mfumo wa usimamizi wa microfinance (wateja, circles/vikundi, mikopo, malipo ya kila siku,
approvals, cashbook, ripoti) — frontend moja ya HTML + Netlify Function moja + Netlify Blobs
kwa ajili ya database ya server.

## Marekebisho yaliyofanywa (v1.0.6 -> v1.0.7)
1. **Bug kubwa:** vitufe vya "+ Circle", "+ Officer" (kwenye ukurasa wa Circles) na "+ User" /
   "Edit" (kwenye ukurasa wa Users) havikufanya kazi kabisa kwa sababu functions zake
   (`openCircle`, `openOfficer`, `openUser`, `saveCircleForm`, `saveUserForm`) hazikuwepo
   kwenye faili — zimeongezwa sasa na zinafanya kazi kikamilifu (create/edit Circle, create/edit
   Users wa mfumo ikiwa ni pamoja na role, password na status active/inactive).
2. Bug ya server: wakati wa ku-save user, `active` status ilikuwa inawekwa `true` kila mara
   hata kama umechagua "Inactive" — imerekebishwa kwenye `netlify/functions/api.js`.
3. Function ya zamani, iliyorudufiwa (`clientsPage` ya kwanza isiyokuwa na Centre/Group) imeondolewa
   ili isilete mkanganyiko — toleo sahihi lenye Centre/Group/Loan Cycle ndilo linalotumika.
4. Code zote mbili (frontend na function) zimepitiwa upya kuhakikisha hazina syntax errors.

## Muundo wa mradi
```
index.html                 -> ukurasa mzima wa mfumo (frontend)
netlify/functions/api.js   -> API moja inayoshughulikia login, data, users
netlify.toml               -> mipangilio ya Netlify (publish + functions folder)
package.json               -> dependency: @netlify/blobs
```

## Environment variables zinazohitajika (Site configuration > Environment variables)
- `SESSION_SECRET` — andika string ndefu ya nasibu (mfano herufi/namba 40+), siri ya kusainia session tokens.
- `NETLIFY_SITE_ID` — Project ID ya site yako (Site configuration > General > Site details > Site ID).
- `NETLIFY_AUTH_TOKEN` — Personal Access Token (User settings > Applications > New access token). Weka kama "Secret".

Baada ya kuweka/kubadili hizi, lazima ufanye deploy mpya (trigger deploy) ili zianze kutumika.

## Njia ya kudeploy (chagua mojawapo)

### Njia A — GitHub + Netlify (inapendekezwa zaidi, rahisi na salama)
1. Tengeneza repo mpya kwenye GitHub, pakia (push) folder hii nzima (isipokuwa huhitaji
   node_modules — Netlify itaisakinisha yenyewe).
2. Kwenye Netlify: **Add new site > Import an existing project** > chagua GitHub > chagua repo.
3. Build settings: acha **Build command** wazi (hakuna build inayohitajika), **Publish directory**
   iwe `.` (au acha default kwani netlify.toml tayari inaeleza hivyo).
4. Kabla ya "Deploy site", au mara baada ya site kuundwa, nenda **Site configuration >
   Environment variables** na weka 3 zilizotajwa juu.
5. Bofya **Deploy site** (au **Trigger deploy > Clear cache and deploy site** kama tayari
   ipo). Netlify itafanya `npm install` yenyewe na kuunganisha function na dependency zake.

### Njia B — Netlify CLI (kama hutaki kutumia GitHub)
Kwenye computer yako (yenye internet na Node.js iliyosakinishwa):
```
npm install -g netlify-cli   # mara moja tu
cd beva-microfinance         # folder yenye faili hizi
npm install                  # muhimu: hii inapakua @netlify/blobs
netlify login
netlify init                 # au: netlify link  (kama site tayari ipo)
netlify deploy --prod
```
`npm install` ni ya lazima kabla ya `netlify deploy` kwa sababu CLI inahitaji dependency
iwepo locally ili iweze kuiunganisha (bundle) kwenye function.

### Kwa nini SIYO drag-and-drop ya moja kwa moja kwenye ukurasa wa "Deploys"
Njia ya kudondosha (drag & drop) folder moja kwa moja kwenye Netlify UI haifanyi `npm install`
kiotomatiki. Ukiitumia bila kuwa na `node_modules` ndani ya folder utakayodondosha, function
`api.js` itashindwa kufanya kazi (haitapata `@netlify/blobs`). Kama lazima utumie njia hii,
lazima kwanza ufanye `npm install` kwenye computer yako na uhakikishe folder ya `node_modules`
inaingizwa ndani ya zip/ folder unayodondosha.

## Watumiaji wa default (badilisha password mara moja baada ya deploy la kwanza)
- admin / admin123 (Administrator — anaweza kuongeza/kuedit users)
- manager / manager123
- officer / officer123 (Loan Officer)
- cashier / cashier123
- viewer / viewer123

Baada ya kuingia kama admin, nenda **Users** kubadilisha password za kila account (au futa/zima
zile hutozitumii kwa kuweka status "Inactive").

## Backup
Ukurasa wa **Reports** na **Settings** una kitufe cha "Backup JSON" / "Download JSON" — pakua
mara kwa mara kama nakala ya ziada, ingawa data halisi inahifadhiwa salama kwenye Netlify Blobs.
