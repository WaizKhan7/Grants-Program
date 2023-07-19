# SmartMuv

- **Team Name:** SmartMuv

## Project Overview :page_facing_up:

### Overview

We propose a system that will facilitate substrate users in the analysis and extraction of the storage state of Ink! smart contracts. It will provide users with a comprehensive view of their smart contract's storage variable values, and facilitate management of contract data. The extracted state will also help in the data upgrade/redeployment of the ink! smart contracts.

#### Objectives

Smartmuv's goal is to offer ink! users: 
- Complete smart contract storage state analysis and extraction, along with the association of state variables and mapping keys with their keys/slots and values.
- Help upgrade their smart contracts with the freedom of changing state variables layout, or any other major changes in the source code.
- Keep integrity and accessibility of data even after changes in variable layout in upgraded contract.
- Easily manage, debug, or audit their contract's state and behavior from storage value perspective.

#### Current Limitations of Upgrade
The current ink upgrade scheme forces its users to maintain the contract’s storage ordering/compatibility in the new code. The user must add the “set_code_hash()” function in its code to upgrade (just like proxies or delegates) but our proposed solution will not be limited to it, and the smart contracts without the “set_code_hash” function will also be upgradeable using the extracted storage state. 

#### Technical Challenge
To extract a smart contract’s complete state, we need to understand and analyze the complete storage structure of Ink contracts. Ink! smart contracts utilize a structured storage layout to manage their state variables. The storage is organized into slots/keys (which are either generated automatically or manually), which act as containers for storing data. Regular state variables are relatively easy to extract as their values can be read through their declared name. However, certain limitations and complexities arise in the case of mapping variables or variables declared as lazy. </br>
Mapping variables in ink! smart contracts pose a challenge during state extraction, as we need keys to extract their values. Additionally, mapping variables with manual keys can be hard-coded in the source code, requiring additional steps to track the keys. These challenges necessitate the development of techniques and algorithms that employ deep analysis techniques to track and provide all possible values of mapping key values to facilitate comprehensive state extraction of all variables.

#### Solution
Our solution will utilize the abstract syntax tree (AST) and control flow graphs (CFG) generated from the source code to extract the value of each contract variable, including mapping type variables across the inheritance hierarchy. With the deep analysis of AST and CFG, our solution will perform key approximation analysis, event analysis, and inter-procedural transaction analysis to extract the contract’s complete state including mapping keys (both auto and manual), and hard-coded variables’ slots/keys defined in the source code.


### Project Details

To extract mapping variables, our solution will employ key approximation analysis. It will use reaching definition analysis, and examine the set of all possible definitions reached by the keys used in mappings. By backtracking over the CFGs of contract functions, Smartmuv will approximate the origin of these keys, determining whether they come from function arguments, events, state variables, or are hard-coded in the source code. Once the origin of the keys is established, their values will be extracted from their respective sources. For example, for keys originating from function arguments, transaction analysis will retrieve all the keys’ values, and event analysis will retrieve keys being passed as part of event arguments. And hard-coded keys can be also directly extracted from the AST.

Smartmuv high-level flow

![Ink_state](https://github.com/WaizKhan7/Grants-Program/assets/67906932/05d099be-39cf-40f1-97d6-47998375f123)


We will also implement inter-procedural analysis that will allow us to backtrack/retrieve key values that are being passed through multiple function calls, and ensure that we are not missing any mapping key. For this purpose, our algorithms will also backtrack every possible path of the key sources.

Below is the complete flow of interprocedural-based mapping keys analysis

![extraction_flow](https://github.com/WaizKhan7/Grants-Program/assets/67906932/4ba7fde1-5bf6-472b-ac5a-f2a87527d7fb)


#### Observation
Currently, this project will focus on the state extraction work while using this state to enable the ink! smart contract upgrades will be done in future work.


### Ecosystem Fit

This project will primarily target developers who work with Substrate and ink! smart contracts, assisting them in building, managing, debugging, and upgrading their contracts on the Substrate blockchain. Its purpose will be to fulfill the need for extracting and analyzing the storage state of ink! smart contracts, providing developers with a comprehensive understanding of the contract's storage layout, including elementary variables, vectors, mappings, structs, and other associated variables. 
There is currently no existing tool that can extract the complete state of ink! smart contracts. Therefore, this project stands out in the ecosystem due to its focus on ink! smart contracts and the utilization of advanced techniques such as key approximation analysis, AST and CFG analysis, and transactions and event analysis. These unique features set it apart from other projects in the ecosystem.


## Team :busts_in_silhouette:

### Team members

- Muhammad Umar Janjua
- Muhammad Waiz Khan
- Maha Ayub

### Contact

- **Contact Name:** Muhammad Waiz Khan
- **Contact Email:** waiz.khan@smartmuv.app
- **Website:** www.smartmuv.app

### Legal Structure

- **Registered Address:** 20 Wenlock Road, London, England, N1 7GU
- **Registered Legal Entity:** Nextkore Ltd.


### Team's experience

We have already developed a Solidity smart contract state analysis and extraction for EVM-compatible blockchains, and have published our [research paper on TOSEM](https://dl.acm.org/doi/10.1145/3548683). The links to the GitHub repo and website are provided below.

### Team Code Repos

- https://github.com/WaizKhan7/SmartMuv

Please also provide the GitHub accounts of all team members. If they contain no activity, references to projects hosted elsewhere or live are also fine.

- https://www.smartmuv.app/ (Website)

### Team LinkedIn Profiles (if available)

- https://www.linkedin.com/in/muhammad-umar-janjua-7493556a/
- https://www.linkedin.com/in/waizkhan1/
- https://www.linkedin.com/in/maha-ayub-48a79713b/

#### Other profiles
- https://scholar.google.com/citations?user=GriAk_8AAAAJ&hl=en

## Development Status :open_book:

We have developed the state extraction platform for Solidity smart contract, which you can find at:
- Publication: [Storage State Analysis and Extraction of Ethereum Blockchain Smart Contracts | ACM Transactions on Software Engineering and Methodology](https://dl.acm.org/doi/10.1145/3548683)
- Code repository: https://github.com/WaizKhan7/SmartMuv/
- Website: https://www.smartmuv.app/
  
## Development Roadmap :nut_and_bolt:

### Overview

- **Total Estimated Duration:** 12 months
- **Full-Time Equivalent (FTE):**  2-3 FTE

### Milestone 1 - Analyze Substrate & Ink smart contracts storage layout


- **Estimated duration:** 1 month
- **FTE:**  2

In this milestone, we will analyze the substrate storage layout to identify how data is stored and organized within the ink! smart contract. We will also analyze the ink! smart contract variables and their interactions with the substrate storage and provide a complete study on ink! smart contract storage.

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | GPLv3  |
| **0b.** | Documentation | We will provide research documentation on substrate and ink! smart contract storage, and how it can be extracted. |
| 1. | Comparison with Ethereum storage and Analyzing Substrate storage layout |We will provide a complete comparison of the smart contract storage layout of Ethereum and Substrate and provide the answers to the following questions: <br /> a) Is the substrate storage extractable? <br /> b) What is the limitation of ink! smart contract storage extraction? <br /> c) How does our proposed technique help in overcoming these limitations? <br /> d) What benefits do we get through storage extraction?|
| 2. | Report | Report on manual inspection of ink! smart contracts.|

### Milestone 2 - Ink! Smart Contract AST Analyzer

- **Estimated Duration:** 2 month
- **FTE:**  2-3
  
In this milestone, we will design an ink! smart contract AST analyzer using existing ink! parsers to identify the state variable, parse the storage slot/key and analyze the ink! smart contract source code.

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | GPLv3  |
| **0b.** | Documentation | We will provide technical documentation and a user guide for the contract analyzer. And also comparison b/w existing technique and our solution.|
| **0c.** | Tests and Test Guide | To ensure functionality, comprehensive unit tests will cover all core functions. We will describe how to run these tests in the guide.|
| 1. | Available parsing tool| Discover and Analyze the different available parsers for ink! or Rust language.|
| 2. | Design and implement the ink! Analyzer | We will design the ink! smart contract analyzer that will parse the source code and perform the AST analysis to provide details of state variables, functions, events, and storage access points within the contract. The analyzer will also help in generating CFG, and call graphs for further analysis.|
| 3. | Automatic Report Generation | A module that will automatically generate a report consisting of details of state variables, functions, and events detected within the contract code. It will be similar to this [report](https://drive.google.com/file/d/1oUyxFpVtCotXfB6dDNGjTAhYp0Ah5MZo/view?usp=sharing) (see first two sections). |
| 4. | Module Testing | The solution will be tested on multiple ink! smart contracts. All results will be provided along with the test smart contract.|

### Milestone 3 - Static Analysis and Extraction of Regular State Variables

- **Estimated Duration:** 1 month
- **FTE:**  2-3

In this milestone, we will write a regular variables extractor. It will perform the static analysis of regular state variables, and automatically extract their values from the blockchain node.

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | GPLv3  |
| **0b.** | Documentation | We will provide the complete technical documentation.|
| **0c.** | Tests and Test Guide | To ensure functionality, comprehensive unit tests will cover all core functions. Details will be provided within the documentation. |
| 1. | Static analysis of regular variables| The module will perform static analysis on the ink! source code and detect regular state variables, and calculates the required storage slots/keys using the ink! parser. <br/> It will provide details of extracted regular state variables i.e. storage key/slot, size, type, etc. aiding in understanding the contract's state storage structure.|
| 2. | Automatic Report Generation | A module that will automatically generate a report consisting of details on all detected regular variables in a contract, including the number of declarations, storage slot allocation, and any dependencies in the provided contract code. <br/> It will be similar to this [report](https://drive.google.com/file/d/1oUyxFpVtCotXfB6dDNGjTAhYp0Ah5MZo/view?usp=sharing) (see “Extracted State Regular Variables” section) |
| 3. | Module Testing | We will test the algorithm on different smart contracts using a diverse dataset. And provide the complete dataset and testing results. |

### Milestone 4 - Static Analysis of Complex Variables and Storage Layout Detection

- **Estimated Duration:** 1 month
- **FTE:**  2-3

We will perform the static analysis of complex variables including arrays, mapping, structs, and enumerations.

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | GPLv3  |
| **0b.** | Documentation | We will provide the complete technical documentation.|
| **0c.** | Tests and Test Guide | To ensure functionality, comprehensive unit tests will cover all core functions. We will describe how to run these tests in the guide. |
| 1. | Static analysis of complex variables | The module will perform static analysis of complex variables within ink! smart contracts and detect their storage layout, including slot declarations. |
| 2. | Automatic Report Generation | This module will automatically generate a report consisting of the details of all detected complex variables in the contract, including the number of declarations, storage slot/key allocation, and any dependencies in the contract code. |
| 3. | Module Testing |We will test the algorithm on different smart contracts using a diverse dataset. And provide the complete dataset and testing results. |


### Milestone 5 - Mapping Structure Analysis and Key Approximation Algorithm for Ink! Smart Contracts

- **Estimated Duration:** 2 month
- **FTE:**  2-3

In this milestone, we will develop a key approximation algorithm that will examine the smart contract functions and generates a list of mapping keys utilized within the contract.

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | GPLv3  |
| **0b.** | Documentation | We will provide the complete technical documentation.|
| **0c.** | Tests and Test Guide | To ensure functionality, comprehensive unit tests will cover all core functions. Details about how to run these tests will be provided in the guide. |
| **0d.** | Article | We will publish an article/tutorial on medium on key analysis of ink! mapping structure. |
| 1. | Ink! Mapping Key Analysis | Complete research on analyzing the Mapping structure of ink! smart contract and how we can extract its keys along with its associated values. |
| 2. | Proposing Key Approximation Algorithm | The module will implement the key approximation algorithm that will analyze the ink! smart contract AST and functions’ CFG and trace back the key set of mapping variables. |
| 3. | Automatic Report Generation | A module that will automatically generate a report consisting of details on mapping variables, code blocks where mapping variables were modified, and detected sources of the keys from the contract code. <br/> It will be similar to this [report](https://drive.google.com/file/d/1oUyxFpVtCotXfB6dDNGjTAhYp0Ah5MZo/view?usp=sharing) (see “Mapping Key Analysis Results” section) but will be more comprehensive. |
| 4. | Module Testing | We will test the algorithm on different smart contracts using a diverse dataset. And provide the complete dataset and testing results. |

### Milestone 6 - Enhancement of Key Approximation Algorithm

- **Estimated Duration:** 1 month
- **FTE:**  2-3

We will expand the key approximation algorithm and add interprocedural analysis and event analysis. This extension aims to maximize the set of keys for mapping state variables. Furthermore, we will incorporate alias analysis to ensure that no keys or mapping slots are overlooked due to local variables.

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | GPLv3  |
| **0b.** | Documentation | We will provide the complete technical documentation.|
| **0c.** | Tests and Test Guide | To ensure functionality, comprehensive unit tests will cover all core functions. Details will be provided within the documentation. |
| 1. | Enhanced Algorithm | Extend the key approximation analysis by implementing interprocedural analysis and the events in smart contract source code. It will also include alias analysis, in case mapping variables are being modified using alias. |
| 2. | Automatic Report Generation | A module that will generate a report based on the key approximation analysis of the provided contract code. The report will contain details of the keys’ source calculated using interprocedural and event analysis, and details of aliases detected. |
| 3. | Module Testing | We will test the module on different smart contracts using a diverse dataset. And provide the complete dataset and testing results. |


### Milestone 7 - Extraction of Mapping Keys

- **Estimated Duration:** 1 month
- **FTE:**  2-3

We will extract the mapping keys that have been analyzed from the transactions and events of the smart contract. This process involves capturing and retrieving the relevant mapping keys that are utilized within the contract's transactions and events.

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | GPLv3  |
| **0b.** | Documentation | We will provide the complete technical documentation.|
| **0c.** | Tests and Test Guide | To ensure functionality, comprehensive unit tests will cover all core functions. Details will be provided within the documentation. |
| 1. | Mapping Keys Extractor | Provide the extraction algorithm that will analyze the transactions and events to get the actual mapping keys using the key approximation analysis information. |
| 2. | Automatic Report Generation | A module that will automatically generate a detailed report containing the actual key set of each associated state mapping variable presented in a structured format. Data could also be used for analysis or upgrade purposes. <br/> It will be similar to this [report](https://drive.google.com/file/d/1oUyxFpVtCotXfB6dDNGjTAhYp0Ah5MZo/view?usp=sharing) (see “Extracted State of Mapping Variables” section), but will be more comprehensive. |
| 3. | Module Testing | We will test the module on different smart contracts using a diverse dataset. And provide the complete dataset and testing results. |


### Milestone 8 - Complete State Extraction of ink! Smart Contracts

- **Estimated Duration:** 1 month
- **FTE:**  2-3

In this milestone, we will provide a complete product that will extract the complete state of the smart contract. This involves obtaining the key/slot information for both regular and complex variables and providing all the values associated with each state variable. The extraction process ensures the comprehensive retrieval of the contract's state, including proper association with the relevant variable values.

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | GPLv3  |
| **0b.** | Documentation | We will provide the complete technical documentation.|
| **0c.** | Tests and Test Guide | To ensure functionality, comprehensive unit tests will cover all core functions of the final product. We will describe how to run these tests in the guide. |
| 1. | State Extractor of ink! Smart Contracts | This module will provide the complete state of the smart contract by reading all values on getting the key set and regular and complex variable slot information. |
| 2. | Automatic Report Generation | A module that will provide a detailed complete automatically generated report of the smart contract. It will contain details of complete AST analysis, key approximation analysis, and the complete extracted state of the contract including all regular and complex variables. <br/> The report will be similar but more detailed and well-structured to this [report](https://drive.google.com/file/d/1oUyxFpVtCotXfB6dDNGjTAhYp0Ah5MZo/view?usp=sharing).|
| 3. | Complete Product Testing | The final product will be tested on different smart contracts and different scenarios. The smart contract dataset used and its validation results will be provided in detail. |


### Milestone 9 - Validation and Soundness of Extracted State with External Explorers

- **Estimated Duration:** 1.5 month
- **FTE:**  2-3

As part of this milestone, we will validate and ensure the soundness of the extracted state by comparing it with existing solutions or explorers. This process involves cross-referencing the extracted state with external sources to verify its accuracy and reliability. By conducting thorough validation, we aim to provide confidence in the correctness of the extracted state for the smart contract.

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | GPLv3  |
| **0b.** | Documentation | We will provide complete documentation on the soundness and validation of extracted state. |
| 1. | Soundness Analysis | Verify the soundness of extracted states. |
| 2. | Validation of Extracted State | Validate the extracted state completeness automatically with the available information on Explorer or any other source. |

## Future Plans

Upon completion of this industry grant, users on the Substrate blockchain will have a product for automatic and easy analysis and extraction of the state and code of ink! smart contracts. The next crucial steps will involve offering data upgrade solutions for ink! smart contracts. Furthermore, we will prioritize user research and rapid feedback cycles to directly incorporate the needs and preferences of the community once the state extraction tool becomes operational within this grant.


