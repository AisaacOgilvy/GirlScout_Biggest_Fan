# Was GTA 5 at fault for the downfall of the Boy Scouts of America?

### And what can the journey to find out teach us about making a TV show about Scouting on Mars?

<img src="Bugling%20at%20the%20Mars%20Scout%20Camp.png" width="603">

The Boy Scouts went bankrupt in 2020, and kids had been earning fewer merit badges for years before that. GTA 5 came out in 2013, right when the drop started. **Did video games pull kids away from Scouting?**

Let's go on a journey to find out.

Everything is in **[`girlscoutcookies.ipynb`](girlscoutcookies.ipynb)**.

## The journey

**1. Pulled the number of badges from the website.** Every year, Scouting America posts an article on onscouting.org with a table of how many of each merit badge kids earned. There's no file to download, only tables on a web page. So I used `pd.read_html`, which grabs every table on a page. Then I printed each table to see what was inside and find the right one:

```python
all_tables_2015 = pd.read_html("https://onscouting.org/2016/03/09/2015-merit-badge-rankings/", header=0)
for i, t in enumerate(all_tables_2015):
    print(i, list(t.columns))
```

- The **2015 article**, table `[1]`, has the number of badges for **2009–2015**.
- The **2020 article**, table `[6]`, has the number of badges for **2016–2020**.

I merged the two tables on the badge name to get **2009–2020**. The first merge gave 157 badges instead of 139, because the website put stars (`First Aid*`) and commas in some names. Cleaning the names fixed it.

**2. Pulled the rankings for after 2020.** After 2020, they stopped posting the number of badges and only posted the **ranking** (#1, #2, #3...). So I pulled the tables from the 2021, 2022, 2023, 2024 and 2025 articles the same way. To compare all the years, I turned my 2009–2020 numbers into rankings too (the badge with the most earned = #1). Then I merged everything into one ranking table, **2009–2025**.

**3. Looked at which badges kids pick.** First Aid is #1 almost every year. Bugling is almost always last.

**4. Saw badges falling since 2012.** From 2.18 million (2012) to 1.74 million (2019), then 1.11 million in 2020 (COVID). Girls joining in 2018–2019 didn't change that.

**5. Tested the GTA 5 idea.** I used government data on how many hours teens (15–19) spend gaming each year. Looking at the plain numbers, gaming and badges seemed linked (−0.63). But comparing how each one **changed year to year**, there was no link (−0.06, not significant). **Hypothesis false. GTA 5 wasn't to blame.** So what was?

**6. Followed the money.** I downloaded the Boy Scouts' IRS tax forms (2009–2024). I tried to get Python to read them, but they're scanned pictures, and pandas can't read pictures. After losing that fight, I listened to Kat (my teacher) and stopped trying to code my way out of it. The numbers went into simple CSV files instead. What I found:

- 2014 was the last year they made money.
- **Insurance + insurance claims went from $53 million (2014) to $220 million (2019)**, more than 3 times what they paid all their staff.

**7. Lined it up with what happened.**

- **2012:** files on abuse by Scout leaders were made public. Badges started falling.
- **2013:** the Scouts voted to allow gay youth members. Many church-run troops left.
- **2014–2019:** abuse lawsuits grew and insurance costs exploded.
- **2020:** bankruptcy, then COVID.

**8. Compared with the Girl Scouts.** Since 2009, the Girl Scouts' net assets grew **70%** while the Boy Scouts' fell **62%**. Girl Scouts spend about **1%** of their money on insurance; in 2019, Boy Scouts spent **51%**.

## What I found

GTA 5 didn't bring down the Boy Scouts. The real causes were inside Scouting: an abuse scandal, lawsuits, huge insurance costs and the bankruptcy that followed.

## Lessons for a Mars Scouts TV show

- **Make episodes about what kids really pick.** First Aid is almost always #1.

  <img src="Cookies/Mars%20Scouts_%20First%20Aid%20Adventure.png" width="500">

- **Try new adventure badges.** Search and Rescue took off fast when it came out.
- **Skip the Bugling episode,** unless it's a joke (like the top picture).
- **Trust matters most.** Kids didn't leave for video games. Parents stopped trusting Scouting.
- **Copy the Girl Scouts:** a fun tradition people love keeps you strong. A Mars Scouts cookie festival is an easy win.

  <img src="Cookies/Martian%20Scout%20Cookies%20Festival.png" width="500">

## Downloads

Get everything with the green **Code → Download ZIP** button. Here's where things are:

| folder | what's inside |
|---|---|
| [`IRS data`](IRS%20data) | Boy Scouts IRS forms (PDFs, 2009–2024), the Boy Scouts CSV files and the teen gaming data |
| [`Girl Scouts data`](Girl%20Scouts%20data) | Girl Scouts IRS forms (PDFs, 2011 and 2023 missing) and CSV files |
| [`Comparison data`](Comparison%20data) | the Boy Scouts vs Girl Scouts insurance CSV |
| [`Cookies`](Cookies) | the First Aid and Cookie Festival pictures (the Bugling picture is in the main folder) |

## Where the data comes from

**Merit badges:** pulled straight from onscouting.org in the notebook with `pd.read_html`: the [2015 article](https://onscouting.org/2016/03/09/2015-merit-badge-rankings/) (table `[1]`), the [2020 article](https://onscouting.org/2021/02/24/2020-merit-badge-rankings-a-list-rewritten-by-the-pandemic/) (table `[6]`), and the 2021–2025 articles (links in the notebook).

**IRS forms (PDFs):** the official tax forms, from the [IRS](https://apps.irs.gov/app/eos/) and [ProPublica](https://projects.propublica.org/nonprofits/). Boy Scouts EIN 22-1576300, Girl Scouts EIN 13-1624016.
**CSV files:**

| file | where the numbers come from | how |
|---|---|---|
| `IRS data/BSA_990_summary.csv` | Boy Scouts IRS forms: money in (line 12), money out (line 18), net assets (line 22). **Page 2** of the PDF for 2009–2015, **page 1** for 2016–2024 | 2012–2022 pulled from [ProPublica](https://projects.propublica.org/nonprofits/organizations/221576300) with Claude's help, the other years read off the PDFs by hand, all checked against the PDFs |
| `IRS data/BSA_990_all_data.csv` | the summary plus donations, fees, salaries and more, 2012–2022 | pulled from [ProPublica](https://projects.propublica.org/nonprofits/organizations/221576300) with Claude's help |
| `IRS data/BSA_990_lawsuit_expenses.csv` | Boy Scouts IRS forms: legal fees (line 11b), insurance (line 23), insurance claims and bankruptcy (line 24), total (line 25). **Page 11** for 2009–2015, **page 10** for 2016–2024 | read off the PDFs by hand, each year checked against line 25 |
| `IRS data/BLS_teen_gaming_hours_2009_2025.csv` | hours per day teens 15–19 spend gaming | pulled from [BLS](https://data.bls.gov/timeseries/TUU10101AA01031055) with Claude's help (no 2020) |
| `Girl Scouts data/GS_990_summary.csv` | Girl Scouts IRS forms: lines 12, 18, 22 (same pages as the Boy Scouts) | 2011–2023 pulled from [ProPublica](https://projects.propublica.org/nonprofits/organizations/131624016) with Claude's help, 2009, 2010, 2024 read off the PDFs by hand |
| `Girl Scouts data/GS_royalties.csv` | Girl Scouts royalty income, 2011–2023 | pulled from [ProPublica](https://projects.propublica.org/nonprofits/organizations/131624016) with Claude's help |
| `Girl Scouts data/GS_990_insurance.csv` | Girl Scouts IRS forms: lines 11b, 23, 25 (same pages as the Boy Scouts) | read off the PDFs by hand, each year checked against line 25 |
| `Comparison data/insurance_comparison.csv` | the two insurance files joined by year | joined by hand |
I first tried to get the numbers out with code, but the IRS forms are scanned pictures and I couldn't make it work. So I gave up on the code and, like Kat (my teacher) suggested, copied the numbers into the CSV files by hand.

The Girl Scouts' year runs October to September, so "2024" means Oct 2023 – Sep 2024.
## Run it yourself

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install pandas matplotlib lxml jupyterlab
jupyter lab girlscoutcookies.ipynb
```

---

*A note on how this README was written: I'm very dyslexic, so I used an AI assistant (Claude) to help put this README together to build the phrame nice and pretty for class, I word vomitted into it help me organize my thoughts and make sure everything was written and shown properly. The research questions, the hypothesis and the conclusions are mine. The data exploration and analysis are mine. :) *
