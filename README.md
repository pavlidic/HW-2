# HPCA_2
## Aristotle Univerity of Thessaloniki

---
* Εργαστήριο Β Ομαδα 3 - 2η Εργασία

---
| **Ονομα**       | ΑΕΜ          |
| --- | -------------- |
| Αιμίλιος Δραγκίνης | 9364 |
| Χρήστος Παυλίδης | 9480 |

---
## Βήμα 1
**1)**
Βασικές παράμετροι του επεξεργαστή που βρίσκουμε στο config.ini:

| **Ονομα** |  **Τιμη** |


|   **Ονομα**| **Τιμη** 
|---|---
|  dcache | size = 65536   
|  icache | size = 32768
|  cache line | size = 64
|  icache associativity |2 
|  dcache associativity|2
|L2 cache| size = 2097152
|L2 associativity|8

Δεδομένου οτι για την προσομοίωση και των 5 bechmarks χρησιμοποιούμε τα ίδια ορίσματα του se.py, τα παραπάνω μεγέθη είναι ίδια για όλα τα config.ini αρχεία και των 5 benchmarks.


**2)**
Tα στοιχεία που αντλούνται απο την προσομοίωση των benchmarks συνοψίζονται στον πρακάτω πίνακα.

|  Aποτελεσμα    | specbzip  | speclibm| specmcf|specsjeng
|---|---|---|---|---
|sim_seconds| 0.083654| 0.174763| 0.062553| 0.513823
|CPI| 1.673085 |3.495270| 1.251067 |10.276466
|miss_Rate DCACHE|0.014312| 0.0060972| 0.002062| 0.121831
|miss_Rate ICACHE|0.000075| 0.000095| 0.019032| 0.000020
|miss_Rate L2cache| 0.295247 |0.999940| 0.067668| 0.999978
---
Στο σημείο αυτό παρατίθενται τα αντίστοιχα διαγράμματα.


![](https://user-images.githubusercontent.com/57071317/101175480-e2c6e780-364d-11eb-9f90-930c5d1c0489.png)

![](https://user-images.githubusercontent.com/57071317/101175495-e6f30500-364d-11eb-9ab5-1bd08530591e.png)

![](https://user-images.githubusercontent.com/57071317/101175509-ebb7b900-364d-11eb-87b7-2e81cfc59141.png)

![](https://user-images.githubusercontent.com/57071317/101175523-ee1a1300-364d-11eb-8bf9-8794df128a6d.png)

![](https://user-images.githubusercontent.com/57071317/101175528-ef4b4000-364d-11eb-87ba-d6b8679f51c5.png0)

![](https://user-images.githubusercontent.com/57071317/101175528-ef4b4000-364d-11eb-87ba-d6b8679f51c5.png)


Απο τα διαγράμματα φαίνεται ότι η πιθανότητα miss στην Icache είναι πολύ μικρή, επομένως αυτη δεν επιβαρύνει τον συνολικο χρόνο εκτέλεσης. Η Dcache εμφανίζει μικρή πιθανότητα miss, εκτός απο την περίπτωση του specsjeng, όπου δεν είναι αμελητέα. Τέλος, για τις περιπτώσεις των specmcf , specsjeng, και speclibm , σχεδόν κάθε φορά που αναζητούνταν ένα δεδομένο στην L2 cache, υπήρχε miss. Οσον αφορα το CPI, όλα τα benchmarks εκτός απο το specsjeng, διατήρησαν χαμηλή την τιμή του. Το μεγάλο CPI σην περίπτωση του specsjeng δικαιολογείται και απο το υψηλο miss rate των caches.

**3)**
To default cpu-clock του se.py έιναι για εμάς 2GHz(το clock του συστηματος ειναι 1GHz). Για τον εντοπισμό των αποτελεσμάτων τρέχουμε τα specsjeng και specmfc με ρολόι 4GHz.

Παρατηρούμε ότι για την περίπτωση των 2GHz έχουμε:
*  System.clk_domain.clock   -->  1000
* System.cpu_clk_domain.clock --> 500

ενω για την περίπτωση των 4GHz έχουμε:
* System.clk_domain.clock --> 1000
* System.cpu_clk_domain --> 250

Aνατρέχοντας στα αρχεία conifig.ini και config.json των προσομοιώσεων με cpu-clock = 4GHZ  και αναζητώντας τις παραπάνω λέξεις παρατηρούμε ότι:

|Xρονίζεται στο 1GHz| Χρονίζεται στα 4GHz
|---|---
|mem_ctrls|cpu
|mem_ctrls.dram|dcache
|dvfs_hundler|icache
||l2cache
||dcache.tags
||icache.tags
||l2.tags
||tol2bus
||cpu.dtb.stage2_mmu
||cpu.itb.stage2_mmu
||cpu.dtb.walker
||cpu.itb.walker

Αν προσθέσουμε αλλον εναν επεξεργαστή ,αυτος θα έχει συχνότητα 4GHz

Για τις προσομοιώσεις με ρολόι 4GHz έχουμε:

||specmfc
|---|--
|sim_seconds|0.032386
|CPI|1.295427
---
||specsjeng
|---|---
|sim_seconds|0.4178
|CPI|16.71256

Διπλασιάζοντας την συχνότητα ρολογιού, παρατηρούμε οτι δεν υποδιπλασιάζεται το sim_seconds (εμφανέστερα στην περίπτωση του specsjeng), οπότε δεν έχουμε τελειο scaling. Παρατηρούμε ότι και στις δυο περιπτώσεις το CPI αυξάνεται. Κατι τέτοιο δικαιολογείται αν σκεφτούμε οτι στην περίπτωση που έχουμε miss στις caches, τα δεδομένα αναζητούνται στην dram της οποιας ο χρονισμος δεν μεταβάλλεται με το ορισμα cpu-clock = 4GHz. Επειδη το memmory accces time της dram ειναι επομένως κοινο και για τις δύο περιπτώσεις ρολογιού, όταν τύχει miss στις caches ο επεξεργαστης θα καθυστερεί το ίδιο χρονικό διάστημα και για τα δυο ρολογια, "χάνοντας" ετσι ανάλογο του clock αριθμό κύκλων επειδη περιμένει να γίνουν fetch τα δεδομένα απο την dram. Ετσι, στις περιπτώσεις όπου έχουμε miss στις cache, δεν υπάρχει τέλειο scalling, η τελειότητα του οποίου μετριάζεται περισσοτερο καθώς το CPI απομακρύνεται απο την τιμή 1 (αυξάνεται to miss_Rate).


## Βήμα 2

Σε αυτο το βημα τρεχουμε τα benchamrks με τις εξης παραμετρους. 
* **cache line**: 32 64 128 (kb)
* **dcache size**: 32 64 128 (kb)
* **ichache size**: 32 64 128 (kb)
* **icache assoc**: 1 2 4
* **dcache assoc**: 1 2 4
* **l2cache** : 512 1024 4096 (kb)
* **l2cache assoc**: 1 2 4 

Εδω παρτίθεται με τη μορφή διαγραμμάτων, η επίδραση των τιμών στο **CPI**

![cache line](https://user-images.githubusercontent.com/57071317/101175464-dfcbf700-364d-11eb-89ba-582174ee68a6.png)


![dcache](https://user-images.githubusercontent.com/57071317/101175485-e490ab00-364d-11eb-825e-9174db90cf86.png)


![dcache assoc](https://user-images.githubusercontent.com/57071317/101175492-e5c1d800-364d-11eb-834a-b6c707cfb1d8.png)


![icache](https://user-images.githubusercontent.com/57071317/101175505-e9edf580-364d-11eb-90e3-ed4fa24d0560.png)

![icache assoc](https://user-images.githubusercontent.com/57071317/101177435-75688600-3650-11eb-94a6-6362d5a7d8d5.jpg)


![l2 cache](https://user-images.githubusercontent.com/57071317/101175513-ec504f80-364d-11eb-8d53-ff0e974b225a.png)


![l2 cache assoc](https://user-images.githubusercontent.com/57071317/101175519-ed817c80-364d-11eb-97e9-b761402b12dc.png)


Απο διαγράμματα συνάγεται το συμπέρασμα ότι μονο η αυξηση του cache line προκαλέι μείωση του CPI.

---

## Βήμα 3

Για την βελτιστοποίηση του παράγοντα κόστους/απόδοσης, χρειάζεται ο καθορισμός μίας συνάρτησης κόστους.  
Σύμφωνα με τα όσα ξέρουμε και την εμπειρία μας:
- Το κόστος της L1 cache / μονάδα μεγέθους είναι σημαντικά μεγαλύτερο από το κόστος / μονάδα μεγέθους της L2 cache.
- Το associativity αυξάνει την πολληπλοκότητα άρα και το κόστος μας κατά έναν παράγoντα κάθε φορά που αυξάνεται.
- Η αύξηση του cache line size αυξάνει επίσης το κόστος αλλά κατα έναν πολύ μικρότερο παράγωντα.


COST = \[(L1 Data Size (kb))x1.1^(L1 Data Assoc) + (L1 Instr Size (kb))x1.1^(L1 Instr Assoc)   
                + (L2 Size (mb))x1.05^(L2 Assoc)\]x1.005^(Cache Line size (b)) 

Comment:  
Τα διαγράμματα ξαναέγιναν σε Matlab ώστε να φαίνονται καλύτερα και να προστεθεί το benchmark spechmmer και άλλαξαν κάποιοι τύποι ώστε να αντιπροσωπεύουν καλύτερα τα ζητούμενα της άσκησης.
