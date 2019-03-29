Steps to Clone & Run Repo in OU's Supercomputer
========

Almost no configuration is necessary because the computer already has R installed on schooner. 

1. Clone repo to local machine.

    ```bash
    git clone https://github.com/wibeasley/RAnalysisSkeleton.git
    cd RAnalysisSkeleton
    ```

1. Check that you can open the desired version of R.  As of March 2019, there are five versions installed on schooner (ranging from 3.3.2 to 3.5.1).  (Thanks to Horst Severini.)

    View the possible R versions with the bash command:
    
    ```bash
    module avail R/
    ```
    
    Set your *personal* default version to 3.5.1 in bash with
    ```bash
    module load R/3.5.1-intel-2016a
    ```
    
    Open R with bash.  Make sure the version is correct (*e.g.*, 3.5.1 if you used the command above).
    ```bash
    R
    ```
    
1. Install a single package.  Install the [devtools](https://CRAN.R-project.org/package=devtools) package.  We'll do this in R (instead of bash), because it more easily establishes a local personal library.
 
    Install package in R.  A lot needs to be compiled, so this take ~4 minutes
    ```R
    install.packages("devtools")
    ```
    
    When prompted, respond
    * `y` to 'Would you like to use a personal library instead? (y/n)'
    * `y` to 'Would you like to create a personal library ~/R/x86_64-redhat-linux-gnu-library/3.3'
    * `1` to select the '0-Cloud [https]' CRAN mirror

1. Install a list of roughly 50 packages from [utility/package-dependency-list.csv](https://github.com/wibeasley/RAnalysisSkeleton/blob/master/utility/package-dependency-list.csv) in R.  Their (package) dependencies are also installed, which takes 15 minutes.

    ```r
    remotes::install_github("OuhscBbmc/OuhscMunge")
    OuhscMunge::package_janitor_remote("utility/package-dependency-list.csv")
    ```
    
    For machines that we have full admin rights, the block above is replaced with `source("utility/install-packages.R")`.  But this approach installs [a longer list of R packages](https://github.com/OuhscBbmc/RedcapExamplesAndPatterns/blob/master/utility/package-dependency-list.csv), which may require additional linux packages.  If you need additional Linux packages installed, contact [OSCER support](http://www.ou.edu/oscer) at support@oscer.ou.edu.
    
1. Three approaches to running all repo scripts with [`utility/reproduce.R`](https://github.com/wibeasley/RAnalysisSkeleton/blob/master/utility/reproduce.R).

    1. Running interactively in R.  This is encouraged only for testing small projects like this.
        ```r
        source("utility/reproduce.R")
        ```
    
    1. Running interactively with Rscript in the bash shell.  This is encouraged only for testing small projects like this.
        ```bash
        Rscript utility/reproduce.R  # This is untested
        ```
    
    1. Run with a bsub file. --Henry, work your magic here.