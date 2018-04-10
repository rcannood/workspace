# workspace

A package for allowing easy management of your project workspace. 
This package assumes that an environment variable `MY_WORKSPACE_PATH` has been set,
and that your project resides within this folder.

For example, the `.bash_profile` contains:
```bash
export MY_WORKSPACE_PATH=/home/user/Workspace
```

It will assume the following structure, in your workspace:

- `$MY_WORKSPACE_PATH/projects`: Your R code
- `$MY_WORKSPACE_PATH/data/raw_data`: Any data you created or download that you certainly never want to delete
- `$MY_WORKSPACE_PATH/data/derived_data`: Data reproducible by scripts and raw data; you should be able to safely delete this folder.
- `$MY_WORKSPACE_PATH/data/result`: Any figures or data objects that you save

An R script located at `/home/user/Workspace/projects/myproject/script.R` could contain the following:
```R
library(tidyverse)
library(workspace)

# you need to call this first, to let workspace know in which project/subdirectory you're working in
project("myproject")

# save data at /home/user/Workspace/data/derived_data/myproject/data.rds
data <- tribble(
  ~x, ~y, 
  1,  2,
  3,  1, 
  7,  2
)
write_rds(data, derived_file("data.rds"))

# save a plot at /home/user/Workspace/data/result/myproject/myplot.pdf
g <- ggpot(data) + geom_point(aes(x, y))
ggsave(result_file("myplot.pdf"), g, width = 10, height = 10)

# read data from a different project, located at /home/user/Workspace/data/derived_data/some_other_project/data.rds
old_data <- read_rds(derived_file("data.rds", "some_other_project"))

# read this script... for whatever reason.
code_txt <- read_lines(code_file("script.R"))
```