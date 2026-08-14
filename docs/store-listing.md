# Chrome Web Store submission

Everything the dashboard asks for, written out so it can be pasted rather than
improvised at submission time. Each answer matches what the code actually does;
if the code changes, change these too.

## Before you submit

- [ ] Replace `SET_CONTACT_EMAIL` in `docs/privacy.html` with a real address.
- [ ] Publish `docs/` on GitHub Pages and paste the resulting privacy URL below.
- [ ] Screenshots, 1280x800 or 640x400, at least one.
- [ ] Store icon: `store/icon-128.png`.
- [ ] Upload `releases/program-list-exporter-<version>.zip`.

## Listing

**Name**

    Program List Exporter for Residency Explorer

**Short description** (129 of 132 characters)

    Export the residency program data you can already see on residencyexplorer.org as a CSV. Not affiliated with or endorsed by AAMC.

**Category:** Workflow & Planning

**Detailed description**

    Turn the program list you are already browsing on Residency Explorer into a
    spreadsheet.

    Sign in to residencyexplorer.org the way you normally do, pick a specialty,
    then open this panel. It reads the programs you have access to, narrows them
    by the filters and rules you set, and writes the result to a CSV file.

    What it does:

    - Filters by region and by visa sponsorship. Choosing several visas keeps a
      program that sponsors any one of them.
    - Applies your own rules to the numbers on each program page, such as a
      Step 2 CK percentile or the share of residents who are Non-US IMGs.
    - Lets you choose which columns the file holds, and in what order.
    - Remembers pages it has already read, so changing a rule and exporting
      again needs no further downloads.

    It works entirely inside your browser. There is no account to create, no
    server involved, and nothing is uploaded anywhere. It never sees your
    password or your sign-in code: you sign in on the site yourself and the
    extension reads pages through that tab.

    This is an independent tool. It is not affiliated with, endorsed by, or
    connected to the AAMC or the Residency Explorer program.

## Privacy practices

**Single purpose**

    The extension exports residency program information that the signed-in user
    can already view on residencyexplorer.org into a CSV file. It has no other
    feature.

**Permission justifications**

`storage`

    Stores the user's own export settings, which filters, criteria and columns
    they picked, so the panel is the same next time they open it. No personal
    information is involved.

`downloads`

    Delivers the finished CSV file to the user. This is the only way the
    extension produces output, and the user chooses where the file is saved.

`sidePanel`

    The entire interface is a side panel shown next to the Residency Explorer
    page. The extension has no other UI.

`host_permissions` for `https://www.residencyexplorer.org/*`

    The extension reads program pages from this one site in order to export
    them. Requests are made from the user's own signed-in tab, and no other
    site is accessed. This permission is the extension's core function; without
    it there is nothing to export.

**Remote code:** No. All code is included in the package.

**Data usage.** Declare no collection in every category. For reference, the
extension does not collect: personally identifiable information, health
information, financial information, authentication information, personal
communications, location, web history, or user activity. Program information
read from the site is cached in the user's own browser and never transmitted
anywhere.

The three certifications are all true as written:

- Data is not sold or transferred to third parties, except for approved use cases.
- Data is not used or transferred for purposes unrelated to the item's single purpose.
- Data is not used or transferred to determine creditworthiness or for lending purposes.

**Privacy policy URL**

    SET_PRIVACY_URL

## Publishing docs/ on GitHub Pages

Push the repository to GitHub, then in Settings, Pages, set the source to the
`main` branch and the `/docs` folder. The policy is then at
`https://<user>.github.io/<repo>/privacy.html`, which is the URL the dashboard
wants.

## Notes on review

The name uses "Residency Explorer" referentially, to say what the extension
works with. Keeping "Not affiliated with or endorsed by AAMC" in the short
description, the detailed description and the privacy policy is what keeps that
from reading as a claim of endorsement.
