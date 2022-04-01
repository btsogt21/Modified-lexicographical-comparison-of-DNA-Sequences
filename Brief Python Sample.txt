import time
import string
import random
from typing import List, Tuple

# Generator function for list of dna sequences. Takes in
# as input the desired number of DNA strings you want in 
# the list of dna strings with the caveat that 
# 0 < # of DNA strings <= 10^6.
# Outputs a list of dna sequences where S_i ∈ {A, T, C, G},
# 0 < len(S_i) <= 10^6, and 0 < len(concatenation of all 
# dna strings in list) < 10^8.

def random_list_of_strings(length: int) -> Tuple[List[str], int]:
    while length>10**6:
        try:
            length = int(input('Please enter an integer length greater than 0 and less than or equal to 10^6 :'))
        except ValueError:
            print('Please enter a valid integer greater than 0 and less than or equal to 10^6')
    i = []
    counter = 0
    for x in range(length):
        curr_rand = random.randint(1, 10**6)
        counter += curr_rand
        if counter>=10**8:
            return_x = x
            break
        i.append(str(''.join(random.choices(['A', 'C', 'T', 'G'], k = curr_rand))))
        return_x = x
    return i, return_x


# Generator function for list containing index pairs as tuples.
# Takes as input desired number of index pairs to insert in list
# as well as the highest index in the list of DNA strings.
# Outputs a list containing tuples which themselves contain
# two integers representing the indexes to be compared.
# Caveat that 0 < tuples in list <= 10^6.
def list_of_indices(length:int, max_index:int) -> List[Tuple[int]]:
    while length>10**6:
        try:
            length = int(input('Please enter an integer length greater than 0 and less than or equal to 10^6 :'))
        except ValueError:
            print('Please enter a valid integer greater than 0 and less than or equal to 10^6')
    i = []
    for x in range(length):
        i.append((random.randint(1, max_index-1), random.randint(1, max_index-1)))
    return i


# Function that takes in list of dna sequences and list of
# index pairs (i, j) and outputs whether or not s_i is
# lexicographically less than s_j for each index pair. 
# Output is in form of list of booleans. Caveat that if 
# len(s_i) < len(s_j), s_i < s_j will evaluate to True.
# However, if len(s_i) is not less than len(s_j), then
# lexicographical comparison will occur as normal. 
# For example,

# 'AG' < 'AAA' will output as 'True' when put through our
# function as 
# len('AG') < len('AAA') despite the fact that, purely
# lexicographically, 'AG' < 'AAA' should output 'False'.

# However,
# 'AAA' < 'AG' will also output as 'True' given that
# len(s_i), in this case, is not less than len(s_j), and
# purely lexicographically, 'AAA' < 'AG' would output 'True'

# less_than_v2 is the version of less_than where if
# len(s_i) > len(s_j), then s_i < s_j evaluates to 'False'
# regardless of what purely lexicographical comparison
# would output. That is, 'AAA' (s_i) < 'AG'(s_j) would 
# output 'False' as len('AG') < len('AAA'), even though,
# purely lexicographically, 'AAA' < 'AG' would output as
# 'True'.
def less_than(s: List[str], q: List[Tuple[int]]) -> List[bool]:
    return [True if len(s[i])<len(s[j]) else s[i]<s[j] for i, j in q]

def less_than_v2(s: List[str], q: List[Tuple[int]]) -> List[bool]:
    return [True if len(s[i])<len(s[j]) else False if len(s[i])>len(s[j]) else s[i]<s[j] for i, j in q]

# Tester function that utilizes the previously described
# functions to test the less_than functions. Takes in as
# input desired # of dna strings in the DNA list to test
# as well as the desired # of index pairs in the list of 
# index pairs to test. Outputs the same type of output as \
# the less_than/less_than_v2 functions. 
def final_tester(length_of_string_list:int, length_of_index_list:int)-> List[bool]:
    list_o_strings, highest_index = random_list_of_strings(length_of_string_list)
    list_o_indices = list_of_indices(length_of_index_list, highest_index)
    return less_than(list_o_strings, list_o_indices)