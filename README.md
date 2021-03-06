# Table of Contents
1. [Problem](README.md#problem)
1. [Approach](README.md#approach)
1. [Program structure](README.md#program-structure)
1. [Run the code](README.md#run-the-code)
1. [Repo directory structure](README.md#repo-directory-structure)
1. [Contact](README.md#contact)


# Problem

Imagine you are a data engineer working for an online pharmacy. You are asked to generate a list of all drugs, the total number of UNIQUE individuals who prescribed the medication, and the total drug cost, which must be listed in descending order based on the total drug cost and if there is a tie, drug name in ascending order. 

# Approach

* Since the output of this problem requires a set of unique drug names, it is appropriate to use a dictionary (hash map) `drug_dict` with key is the `drug_name`. Further more, since we only count the number of UNIQUE prescribers for each drug, it is reasonable to use a dictionary for each `drug_name`. These dictionaries have the tuple `pres_name` (created by concatinating `prescriber_last_name` and `prescriber_first_name`) as key. 

    * The program scans through each line of the txt input file. For each line it processes the string to obtain the `prescriber_last_name`, `prescriber_first_name`, `drug_name` and `drug_cost` information. 

    * It then checks if `drug_name` is in `drug_dict`. If it's not, add an item with key is `drug_name` and value is a list of 2 elements: first is a dict with `pres_name` as key, and second is an int variable `cost` (we can do float too, but then there would be issue with the first test file, so I simply round all float values to integers). If `drug_name` is in `drug_dict`, update the total cost by adding `cost`, and add a `pres_name:None` key-value pair to the inner dictionary `pres_dict` if `pres_name` is not there yet. 

    * Since the cost of looking up to see if a key exists in a dictionary is `O(1)`, assume that we have `n` lines in the input txt file, then the cost of this whole block is `O(n)`.

* Next, I copy the `drug_name`, `total_cost` (total cost of a drug), and `len(pres_dict)` (number of distict prescribers for each drug) to a list and sort it by `cost` (ascending order) and `drug_name` (descending order). In order to do this, I used a helper function `cost_name_compare` for sorted . This guarantees that if there is a tie in cost, drug name is in sorted order. 
The time complexity for this block is `O(klog(k))`, where `k` is the number of distinct values `drug_name`.

    * I then use the sorted list above to write to output file. This takes `O(k)`, so in general, the runtime of my program is either `O(n)` or `O(klog(k))`, whichever greater, with `n` is the number of lines in the input txt file, and `k` is the number of distinct values `drug_name`.

# Program structure

My program was broken down into the following functions:
* `get_file_path()`: Get input and output files. Notify an error if either not enough arguments are entered or cannot open input file
* `extract_info(line)`: Get `drug_name`, `cost`, and `pres_name` from each line of the txt file
* `sum_total_cost(pres_name, drug_name, cost, drug_dict)`: Construct the dictionary `drug_dict` and the inner dictionary `pres_dict` 
* `sort_drug(drug_dict)`: Order the dictionary by `drug_name` and `total_cost`
* `cost_name_compare(drug1, drug2)`: helper function for `sort_drug`, compares 2 drugs by total_cost, if they have equal total_cost then compare their drug_name. `drug1` is "smaller" than `drug2` if it has a larger total_cost, or if their costs are equal and drug1 has a earlier dictionary name order
* `main()`: Call `get_file_path()` so get the txt file contents, then scans through each line. For each line it processes the string to obtain the `prescriber_last_name`, `prescriber_first_name`, `drug_name` and `drug_cost` information and print "Line is corrupt" if it can't get one or more fields. Then it calls `sum_total_cost()` to construct the dict and `ord_cost()` to sort it. Finally it writes the output to output txt file.

# Run the code

Either type `run.sh` or `python3 ./src/medicine_count.py ./input/itcont.txt ./output/top_cost_drug.txt` in your Terminal

# Repo directory structure

The directory structure for my submission:

    ├── README.md 
    ├── run.sh
    ├── src
    │   └── pharmacy-counting.py
    ├── input
    │   └── itcont.txt
    ├── output
    |   └── top_cost_drug.txt
    ├── insight_testsuite
        └── run_tests.sh
        └── tests
            └── test_1
            |   ├── input
            |   │   └── itcont.txt
            |   |__ output
            |   │   └── top_cost_drug.txt
            ├── your-own-test_1
                ├── input
                │   └── your-own-input-for-itcont.txt
                |── output
                    └── top_cost_drug.txt
