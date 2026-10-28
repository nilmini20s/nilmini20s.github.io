---
title: "How-To Guide: Rolling Up Simulation Results"
date: 2026-07-15
summary: "A procedural guide for consolidating CPU simulator output files into a single CSV for analysis."
image: "/images/card-howto.svg"
tags: ["How-To Guide"]
---

<div class="about-callout">

## About this writing sample

I wrote this how-to guide from memory, based on my work at Intel. At Intel, I worked frequently with a CPU architectural simulator, running experiments that varied things like CPU frequency, cache size, and credits. Oftentimes, we would run these experiments across hundreds of benchmarks and parameter combinations, resulting in thousands of individual runs, each generating its own output file containing thousands of statistics. 

In order to extract data from these output files, we used the rollup script (a Perl script originally written by another engineer on the team) that aggregated just the metrics we cared about from thousands of files into a single CSV file. From there, we'd do analysis in Excel, such as plotting the data, observing trends, or debugging outliers. 

This roll-up script was something I used very frequently, sometimes multiple times a week. I sometimes showed other engineers on other teams how to do the rollup. No one had documented this process, so I wrote the original instructions on an internal Confluence wiki page. This writing sample recreates that guide from memory, having run this process several hundred times myself. I wrote this sample in Markdown in VS Code and pushed it to Github.

**Intended audience:** Performance validation engineers who have run experiments on the simulator and need to consolidate the results for analysis.

[View source on GitHub](https://github.com/nilmini20s/how-to-guide-writing-sample)

</div>

# How to rollup simulation results into a CSV file

This guide will walk you through how to rollup results files (`stats.gz` files) from your experiments on the CPU architectural simulator. 

This guide assumes that you have already downloaded the simulator repo, launched an experiment with several simulations, and that those runs are complete, and their results are ready to be rolled up.

## Prerequisites

Before proceeding, confirm that your runs completed successfully and that your `results/` folder contains `stats.gz` files. If there are no such files, then your runs most likely failed and you need to relaunch simulations.

In this guide, you will learn how to 
- Download the `scripts` repo which has the rollup scripts 
- Add metrics that you want to extract from the `stats.gz` files
- Rollup the metrics into a CSV file that you can use later for further analysis

**NOTE**: This guide uses the _cpu.arch.4\_0_ project and a core frequency varying experiment for illustration.

## Rollup results

1. Download the `scripts` repo, if you haven't done so already
    1. In a separate folder outside of the simulator repo, clone the scripts repo

        ```bash
        git clone https://github.com/simulator-project/scripts.git
        ```

        **NOTE**: The rollup script is located at `scripts/performance/rollup.pl`

    2. Add any metrics you'd like to rollup into the metrics file. The file already has some common metrics included.

        The metrics file is located at `scripts/performance/cpu.arch.4_0.metrics.txt`

        Examples of metrics you can add

        ```
        # Direct extraction
        cpu_cycles, 		        cpu.cycles

        # Summations	
        total_instrs, 			    sum(cpu.core\d*\.instructions)
        
        # Averages
        average_instrs_per_core, 	avg(cpu.core\d*\.instructions)

        # Formulas
        instrs_per_cycle,		    eqn=total_instrs/cpu_cycles
        ```

2. Save the path to your scripts folder in an environment variable

    ```bash
    # if using bash shell
    export SCRIPTS_PATH=<path to scripts folder>
    # if using tcsh/csh shell
    setenv SCRIPTS_PATH <path to scripts folder>
    ```  

    For example,

    ```bash
    # if using bash shell
    export SCRIPTS_PATH=/home/projections/scripts
    # if using tcsh/csh shell
    setenv SCRIPTS_PATH /home/projections/scripts
    ```

3. Navigate to where the simulator run results are

    ```bash
    cd simulator/cpu.arch.4_0/results/
    ```

    For example, if you ran an experiment varying core frequency from 2GHz to 3.5GHz in increments of 0.5GHz and saved the results in an output folder called `frequency_experiment`, your four stats files are inside this folder like so:  

    ```bash
    find ./ -name "stats.gz"
    ```

    Output:
    ```
    ./frequency_experiment/frequency_2.0_ghz/stats.gz
    ./frequency_experiment/frequency_2.5_ghz/stats.gz
    ./frequency_experiment/frequency_3.0_ghz/stats.gz
    ./frequency_experiment/frequency_3.5_ghz/stats.gz
    ```

4. Call the results script from here
		
    ```bash
    $SCRIPTS_PATH/performance/rollup.pl --metrics $SCRIPTS_PATH/performance/cpu.arch.4_0.metrics.txt --batch all --stats <relative/path/to/stats/files>  --output <my_rollup_file.csv>
    ```

    For example, to rollup our frequency\_experiment

    ```bash
    $SCRIPTS_PATH/performance/rollup.pl --metrics $SCRIPTS_PATH/performance/cpu.arch.4_0.metrics.txt --batch all --stats frequency_experiment/ --output frequency_experiment.csv
    ```

    `--batch` instructs the script to distribute the stats files amongst all the machines listed inside the `rollup.pl` script in order to run the job faster (especially useful if you have hundreds of `stats.gz` files). We typically run with the `--batch` parameter. You can choose to omit it, in which case, your script will run only on your machine. 

    **WARNING**: Occasionally, the script will error out if one of the machines is down (undergoing maintenance). In that case, edit the `rollup.pl` file to temporarily comment out the machine name causing the error.

    ```perl
    		batch_machines = {
			    sc09.plxch09_001,
			    sc09.plxch09_002,
			    sc09.plxch09_003,
			    #sc09.plxch09_004,  #machine unavailable
			    sc09.plxch09_005,
			    sc09.plxch09_006,
			    sc09.plxch09_007,
			    sc09.plxch09_008,
			    sc09.plxch09_009 }
    ```


5. Wait until the batched jobs are all complete and the command line returns. This could take several minutes. If you have more than 500 stats files, this could take 30 minutes or more.

6. Finally, find the CSV file in the same directory you ran the script from, which in this case is the `results/` directory.

     `simulator/cpu.arch.4_0/results/frequency_experiment.csv`

## Next steps

You can WinSCP this CSV file from the ECM (engineering compute machines) to your laptop and open it in Excel to view it. The rows represent each unique stats file. The columns represent each metric extracted.
		
With Excel, you can do further analysis like sorting the data, plotting the data, and identifying outliers or trends. 

**TIP**: If you make edits, remember to _Save As_ a `.xlsx` file.
