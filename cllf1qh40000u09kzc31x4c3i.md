---
title: "Building a Simple Domain Verification Tool: Introducing GoVerifyDomain 🌐"
seoTitle: "Go verify Domain"
seoDescription: "Domain verification involves the process of confirming the authenticity and legitimacy of a domain name address or domain before considering it trustworthy."
datePublished: Thu Aug 17 2023 10:56:22 GMT+0000 (Coordinated Universal Time)
cuid: cllf1qh40000u09kzc31x4c3i
slug: building-a-simple-domain-verification-tool-introducing-goverifydomain
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1692269579147/b9105a78-cc91-4cab-9a4b-055b8fbe16c9.png
tags: go, projects, devops, domain, 2articles1week

---

In today's digital landscape, ensuring the authenticity and security of online communications has become increasingly important. One crucial aspect of this is verifying the legitimacy of domain names. Domain verification helps prevent phishing attacks, spoofing, and other malicious activities by confirming that the claimed sender's domain is legitimate. In this tutorial, we will introduce you to a simple domain verification tool called "GoVerifyDomain." We'll explore the terms and concepts involved in domain verification and guide you through the process of building this tool step by step.

## Understanding Domain Verification

Domain verification involves the process of confirming the authenticity and legitimacy of a domain name address or domain before considering it trustworthy. This verification process is essential to prevent spam, phishing, and other malicious activities. Domain verification primarily revolves around examining DNS records associated with it.

The key components involved in email verification include:

1. **Domain**: A domain is a human-readable address used to access websites and send emails. For example, "[example.com](http://example.com)" is a domain.
    
2. **MX Record**: MX (Mail Exchange) records are DNS records that specify the mail servers responsible for receiving email messages on behalf of a domain. These records are essential for email delivery.
    
3. **TXT Record**: TXT records are general-purpose text-based DNS records used to provide additional information about a domain. They are often used for SPF and DMARC configurations.
    
4. **SPF (Sender Policy Framework)**: SPF is an email authentication protocol that helps prevent email spoofing by allowing domain owners to specify which mail servers are authorized to send emails on behalf of their domain.
    
5. **DMARC (Domain-based Message Authentication, Reporting, and Conformance)**: DMARC is a policy framework that builds upon SPF and DKIM (DomainKeys Identified Mail) to provide better email authentication and reporting. It helps prevent domain spoofing and phishing attacks.
    

## Introducing GoVerifyDomain

GoVerifyDomain is a command-line tool developed in the Go programming language that simplifies the process of verifying domains. The tool utilizes DNS lookups to retrieve and analyze the necessary records, providing valuable insights into the authenticity of the domain. Let's explore how GoVerifyDomain works and how each term is implemented within the tool.

## How GoVerifyDomain Works

The core functionality of GoVerifyDomain revolves around the `net` package in Go, which allows for DNS lookups to retrieve information about a domain's MX, SPF, and DMARC records. Let's break down the key components of the provided code snippet:

1. The program accepts user input for a domain name to be verified. It then initiates the verification process.
    
2. For each domain, the `checkDomain` function is called. This function performs the following steps:
    
    * It uses `net.LookupMX` to retrieve MX records for the domain, checking if there are any mail exchange servers associated with the domain.
        
    * It uses `net.LookupTXT` to retrieve TXT records for SPF and DMARC verification. The program looks for records starting with "v=spf1" to identify SPF records and "v=DMARC1" to identify DMARC records. If found, it indicates that the domain has SPF or DMARC policies in place.
        
3. Once the DNS lookups are performed, the program prints the results, including whether the domain has MX, SPF, or DMARC records, along with the content of the SPF and DMARC records if present.
    

# Let's build

## Prerequisites

Before you begin, make sure you have/know the following:

1. Basic understanding of Go programming concepts.
    
    > You can refer to this repo for Basics: [https://github.com/ChetanThapliyal/get-started-with-Go/tree/main](https://github.com/ChetanThapliyal/get-started-with-Go/tree/main)  
    > OR  
    > Read the Go blog Series:
    > 
    > [https://tech-transitions.hashnode.dev/series/learn-golang](https://tech-transitions.hashnode.dev/series/learn-golang)
    
2. Go programming language installed on your machine. You can download and install Go from the official website: [https://golang.org/dl/](https://golang.org/dl/)
    

### Step 1: Setting Up the Project

1. Create a new directory for your project:
    
    ```sh
    mkdir verify-Domain
    cd verify-Domain
    ```
    
2. Inside the project directory, create a file named `main.go`:
    
    ```sh
    touch main.go
    ```
    

### Step 2: Writing the Go Code

1. Open `main.go` in a text editor and copy the following code:
    
    ```go
    package main
    
    import (
    	"bufio"
    	"fmt"
    	"log"
    	"net"
    	"os"
    	"strings"
    )
    
    func main() {
    	// Initialize a scanner to read input from the user
    	scanner := bufio.NewScanner(os.Stdin)
    
    	// Loop to repeatedly ask for input until "exit" is entered
    	for {
    		fmt.Print("Enter a domain name to verify (or type 'exit' to quit): ")
    		scanner.Scan()
    		input := scanner.Text()
    
    		// Exit the loop if user enters "exit"
    		if input == "exit" {
    			fmt.Println("Exiting...")
    			break
    		}
    
    		// Call the function to check the domain/email
    		checkDomain(input)
    	}
    
    	// Check for any errors while reading input
    	if err := scanner.Err(); err != nil {
    		log.Fatalf("Error: could not read from input: %v\n", err)
    	}
    }
    
    func checkDomain(domain string) {
    	var hasMX, hasSPF, hasDMARC bool
    	var spfRecord, dmarcRecord string
    
    	// Lookup MX records for the domain
    	mxRecords, err := net.LookupMX(domain)
    	if err != nil {
    		log.Printf("Error looking up MX records for %s: %v\n", domain, err)
    	}
    
    	// Check if MX records were found
    	if len(mxRecords) > 0 {
    		hasMX = true
    	}
    
    	// Lookup TXT records for the domain
    	txtRecords, err := net.LookupTXT(domain)
    	if err != nil {
    		log.Printf("Error looking up TXT records for %s: %v\n", domain, err)
    	}
    
    	// Loop through TXT records to find SPF record
    	for _, record := range txtRecords {
    		if strings.HasPrefix(record, "v=spf1") {
    			hasSPF = true
    			spfRecord = record
    			break
    		}
    	}
    
    	// Lookup TXT records for DMARC
    	dmarcRecords, err := net.LookupTXT("_dmarc." + domain)
    	if err != nil {
    		log.Printf("Error looking up DMARC records for %s: %v\n", domain, err)
    	}
    
    	// Loop through DMARC records to find DMARC record
    	for _, record := range dmarcRecords {
    		if strings.HasPrefix(record, "v=DMARC1") {
    			hasDMARC = true
    			dmarcRecord = record
    			break
    		}
    	}
    
    	// Print the results
    	fmt.Println(domain)
    	fmt.Printf("hasMX : %v\n", hasMX)
    	fmt.Printf("hasSPF : %v\n", hasSPF)
    	fmt.Printf("spfRecord : %s\n", spfRecord)
    	fmt.Printf("hasDMARC : %v\n", hasDMARC)
    	fmt.Printf("dmarcRecord : %s\n", dmarcRecord)
    	fmt.Println()
    }
    ```
    

### Step 3: Running the Code/Tool

1. Open a terminal window and navigate to your project directory.
    
2. Run the following command to build and run the web server:
    
    ```sh
    go run main.go
    ```
    
    You should see something like this:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692264144841/545dea2b-5273-476f-915b-d36ce39ecb8d.png align="left")
    
    Enter the domain name, for example `github.com` :
    
    ![Add the domain name:](https://cdn.hashnode.com/res/hashnode/image/upload/v1692263998571/4cd1ce40-d2d4-41a0-b729-c2af6d8be604.png align="left")
    

### Understanding The Code

The provided code snippet demonstrates the core functionality of GoVerifyDomain. The tool prompts the user to enter a domain name and then checks the corresponding MX, SPF, and DMARC records. Let's break down the code step by step.

1. **Importing Necessary Packages**: The tool uses the
    
    `bufio`: Provides buffered I/O functions for input and output operations.
    
    `fmt`: Implements formatted I/O for console output.
    
    `log`: Offers a simple logging package for writing logs
    
    `net`: Offers networking functions, including DNS lookups and network connections.
    
    `os`: Provides operating system functionalities, including file operations.
    
    `strings`: Implements string manipulation and searching functions.
    
2. **Main Function**: The `main` function serves as the entry point of the program:
    
    * It initializes a scanner to read input from the user.
        
    * It enters a loop that repeatedly asks the user to input a domain name to verify until the user enters "exit."
        
    * Within the loop, it reads the input using the scanner and stores it in the `input` variable.
        
    * If the user enters "exit," the program prints a message and exits the loop.
        
    * Otherwise, it calls the `checkDomain` function to perform the verification.
        

### Detailed Understanding of Functions

### `checkDomain` function

This function does the following:

1. It starts by declaring variables (`hasMX`, `hasSPF`, `hasDMARC`) to track whether certain attributes are present for the given domain. It also declares `spfRecord` and `dmarcRecord` to store corresponding records.
    
2. It looks up MX records for the domain using the `net.LookupMX` function, which is like asking the internet where the domain's emails are sent.
    
3. It checks if MX records were found; if yes, it sets the `hasMX` variable to `true`.
    
4. It looks up TXT records for the domain, which can contain various types of information, like authentication details.
    
5. It loops through the retrieved TXT records to find an SPF record. If such a record is found (indicating email sender authentication), it sets `hasSPF` to `true`, and stores the SPF record in `spfRecord`.
    
6. It looks up DMARC records for the domain, which help control email handling policies.
    
7. It loops through the DMARC records to find a DMARC record. If found, it sets `hasDMARC` to `true` and stores the DMARC record in `dmarcRecord`.
    

Finally, it prints the domain's verification results, including whether MX, SPF, and DMARC attributes were found, along with the actual SPF and DMARC records if available.

### **Understanding the Output:**

* The tool's output will include the domain name you entered, whether it has MX records, whether it has SPF records, and if DMARC records are present.
    
* If SPF or DMARC records are found, the tool will display their content.
    

### **Interpreting Results:**

* A "hasMX" result of `true` indicates that the domain has mail servers configured (MX records).
    
* A "hasSPF" result of `true` indicates that the domain has SPF records for sender authentication.
    
* A "hasDMARC" result of `true` indicates that the domain has DMARC records for email handling policies.
    
* The "spfRecord" and "dmarcRecord" results display the actual records found.
    

## Conclusion

Congratulations to all those who have successfully journeyed through the development of "GoVerifyDomain" and have implemented this insightful tool! Your dedication and effort have led you to a better understanding of domain verification, an essential aspect of cybersecurity. By now, you've not only grasped the mechanics of DNS queries and records but have also taken a step forward in enhancing digital communication security. Your accomplishment demonstrates your commitment to learning and your readiness to tackle real-world challenges in the tech landscape.

As you move forward, armed with the knowledge and practical experience gained from this endeavor, remember that even the simplest tools can pave the way for more profound discoveries. The road ahead holds limitless opportunities to dive deeper into cybersecurity, contribute to online safety, and shape the future of technology. Whether you're a newcomer or a seasoned coder, your journey has just begun, and we look forward to witnessing your continued growth and innovation.