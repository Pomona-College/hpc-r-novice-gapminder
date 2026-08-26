---
title: Setup
---

# R for Reproducible Scientific Analysis - Setup Guide

This workshop teaches you how to use R and RStudio for reproducible data analysis, focusing on the Gapminder dataset. You have two setup options: use R on Pomona's Sagehen HPC cluster, or install R locally on your computer.

## Option 1: Sagehen HPC (Recommended for Workshop)

Using R on Sagehen is ideal for this workshop because:
- Pre-installed software (no installation needed)
- Shared Gapminder data files available
- Easy access via web browser
- Collaborative environment

### Access R on Sagehen via OnDemand

#### Step 1: Log into OnDemand

1. Open browser: https://ondemand.hpc.pomona.edu/
2. Log in with Pomona credentials (Duo authentication required)
3. Click "Interactive Apps" in the top menu
4. Select "RStudio Server" (or "RStudio" if available)

#### Step 2: Request RStudio Session

When prompted, enter:
- **Number of hours**: 2-4 (for workshop)
- **Memory**: 8 GB (minimum)
- **CPU cores**: 2-4
- Click "Launch"

Wait 30-60 seconds for the RStudio window to appear.

#### Step 3: Verify RStudio Works

In the RStudio console, type:

```r
# Check R version
R.version
# Should show R version 4.x or later

# Check RStudio
rstudioapi::getVersion()
# Should show RStudio version
```

You're ready for the workshop!

#### Ending Your Session

After the workshop:
1. Click "Home" (top-left)
2. Under "My Interactive Sessions", click "Delete"
3. Your session closes and compute time stops

---

## Option 2: Local Installation (If You Prefer)

If you prefer R on your personal computer instead of Sagehen HPC:

### Install R

Download and install the latest version of R from https://www.r-project.org/:

- **Windows**: Click "Download R for Windows" → "base" → Download the installer
- **Mac**: Click "Download R for (Mac)" → Choose your processor type (Intel or Apple Silicon)
- **Linux**: Follow distro-specific instructions or use package manager:
  - Ubuntu: `sudo apt install r-base`
  - Fedora: `sudo dnf install R`

### Install RStudio

[Download RStudio Desktop (free version)](https://www.rstudio.com/products/rstudio/download/) for your operating system.

RStudio is an IDE that makes R much easier to use.

### Test Your Installation

After installing R and RStudio:

1. Open RStudio
2. In the console (lower-left), type:

```r
R.version
# Should show R 4.x or later
```

---

## Workshop Setup Verification

### For Sagehen HPC Users

Verify you have access to workshop materials:

```r
# Check workshop materials location
list.files("/data/gapminder/")
# Should show: gapminder.txt, continents.csv, or similar

# Check what files are available
dir("/data/gapminder/")
```

If files exist, you can use Sagehen for the workshop. If not, contact its-hpc@pomona.edu.

### For Local R Installation

You don't need pre-existing files; you'll download Gapminder data during the workshop.

---

## Loading Modules on Sagehen HPC

If using Sagehen via terminal (not OnDemand RStudio), load R modules:

```bash
# SSH into Sagehen
ssh <myusername>@sagehen.hpc.pomona.edu

# Check available R versions
module avail R
# Shows: r/4.2.2, r/4.2.3, r/4.4.1, r/4.5.1 (D)

# Load R (module names are lowercase)
module load r/4.5.1  # (or just `module load r` for the default)

# Start R
R

# In R console
> R.version
# Verifies it works
```

---

## Installing Packages on Sagehen HPC

For Sagehen users, install workshop packages in user space:

```r
# In R console (started via `module load r`)

# Set user library path
dir.create(Sys.getenv("HOME"), ".R/library", recursive = TRUE)
.libPaths(file.path(Sys.getenv("HOME"), ".R/library"))

# Install ggplot2 (for visualization)
install.packages("ggplot2", repos = "http://cran.r-project.org")
# Answer 'yes' to "Create a personal library in..." if prompted

# Install tidyverse (collection of useful packages)
install.packages("tidyverse", repos = "http://cran.r-project.org")

# Verify installation
library(ggplot2)
library(dplyr)
# Should load without errors
```

---

## Downloading Gapminder Data

### Via Sagehen HPC (OnDemand RStudio)

The Gapminder data is already available at:

```r
# Read Gapminder data
gapminder <- read.csv("/data/gapminder/gapminder.csv")

# View first few rows
head(gapminder)

# Check dimensions
dim(gapminder)
# Should show: 1704 rows x 6 columns
```

If the file path doesn't work, download it from:

```r
# Download from official source
url <- "https://raw.githubusercontent.com/swcarpentry/r-novice-gapminder/gh-pages/data/gapminder_data.csv"
gapminder <- read.csv(url)
```

### Via Local R Installation

Download the Gapminder data file:

```r
# In RStudio console
url <- "https://raw.githubusercontent.com/swcarpentry/r-novice-gapminder/gh-pages/data/gapminder_data.csv"
download.file(url, "gapminder_data.csv")

# Read it into R
gapminder <- read.csv("gapminder_data.csv")

# Verify
head(gapminder)
dim(gapminder)
```

Alternatively, [download the CSV directly](https://raw.githubusercontent.com/swcarpentry/r-novice-gapminder/gh-pages/data/gapminder_data.csv) and open in RStudio.

---

## RStudio Configuration

### Default Settings (Usually Fine)

RStudio works well with default settings. Optional improvements:

**Tools → Global Options:**
- **General**: Uncheck "Save workspace to .RData"
- **Code → Editing**: Enable "Soft-wrap R source files" (easier reading)
- **Appearance**: Choose your preferred theme (light/dark)
- **Pane Layout**: Default layout works; customize if preferred

### Setting Working Directory

In RStudio, set where your data files are:

```r
# Check current working directory
getwd()

# Set to your data folder
setwd("~/data/")  # or your actual path

# Verify
list.files()
# Should show your data files
```

---

## Troubleshooting

### "Packages won't install on Sagehen HPC"

If you get permission errors:

1. Create user library directory:
```r
dir.create(file.path(Sys.getenv("HOME"), ".R"), showWarnings = FALSE)
dir.create(file.path(Sys.getenv("HOME"), ".R/library"), showWarnings = FALSE)
```

2. Set library path before installing:
```r
.libPaths(file.path(Sys.getenv("HOME"), ".R/library"))
install.packages("ggplot2")
```

### "Can't find Gapminder data"

- Check file path: `list.files("/data/gapminder/")` or `list.files(".")`
- Download from GitHub (command shown above)
- Contact its-hpc@pomona.edu if /data/gapminder not available

### "RStudio won't start on OnDemand"

- Check internet connection
- Try refreshing the page
- Check available compute resources (may need to wait)
- Contact its-hpc@pomona.edu

### "R version is too old"

Sagehen may have multiple R versions. Request newer one:

```bash
module avail r
# See available versions: r/4.2.2, r/4.2.3, r/4.4.1, r/4.5.1 (D)
module load r/4.5.1  # Load desired version
```

---

## Before the Workshop

Verify your setup is working:

**For Sagehen users**:
- [ ] Can log into OnDemand (https://ondemand.hpc.pomona.edu)
- [ ] Can launch RStudio from Interactive Apps
- [ ] Can type `R.version` in console and see R 4.x
- [ ] Can see `/data/gapminder/` data files (or download them)
- [ ] Can load `ggplot2` package

**For local R users**:
- [ ] R installed from https://www.r-project.org/
- [ ] RStudio Desktop installed
- [ ] Can open RStudio and see R console
- [ ] Can download Gapminder CSV file
- [ ] Can read CSV with `read.csv("gapminder_data.csv")`

---

## Getting Help

- **Sagehen issues**: Email its-hpc@pomona.edu
- **R/RStudio questions**: Office hours (see workshop info)
- **Data questions**: Ask instructor during workshop
- **Debugging**: Use `?function_name` in R to see help

---

## Next Steps

Once setup is complete, you're ready to start the workshop!

**Episode 1: Introduction to R and RStudio** walks through the basics and uses Gapminder data to teach fundamental R concepts.

**Let's learn reproducible data analysis!**

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
