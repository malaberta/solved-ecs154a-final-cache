Download Link: https://assignmentchef.com/product/solved-ecs154a-final-cache
<br>
Implement a Cache that meets the specifications below.

<h1>Cache Specifications</h1>




<table width="624">

 <tbody>

  <tr>

   <td width="312">Parameter</td>

   <td width="312">Value</td>

  </tr>

  <tr>

   <td width="312">CPU Address</td>

   <td width="312">10 bits</td>

  </tr>

  <tr>

   <td width="312">Cache Data Size</td>

   <td width="312">24 Bytes</td>

  </tr>

  <tr>

   <td width="312">Number of Sets</td>

   <td width="312">2</td>

  </tr>

  <tr>

   <td width="312">Number of Ways (Blocks per Set)</td>

   <td width="312">3</td>

  </tr>

  <tr>

   <td width="312">Block Size</td>

   <td width="312">4 Bytes</td>

  </tr>

  <tr>

   <td width="312">Write Policy</td>

   <td width="312">Write Back</td>

  </tr>

  <tr>

   <td width="312">Eviction Policy</td>

   <td width="312">LRU</td>

  </tr>

 </tbody>

</table>

<h1>Inputs</h1>




<table width="624">

 <tbody>

  <tr>

   <td width="118">Input Pin</td>

   <td width="85">Size (bits)</td>

   <td width="421">Description</td>

  </tr>

  <tr>

   <td width="118">CpuAddress</td>

   <td width="85">10 bits</td>

   <td width="421">The address the CPU wants to read from or write to</td>

  </tr>

  <tr>

   <td width="118">CpuRead</td>

   <td width="85">1 bit</td>

   <td width="421">1 If the CPU wants to Read and 0 otherwise</td>

  </tr>

  <tr>

   <td width="118">CpuWrite</td>

   <td width="85">1 bit</td>

   <td width="421">1 if the CPU wants to Write and 0 otherwise</td>

  </tr>

  <tr>

   <td width="118">CpuWriteValue</td>

   <td width="85">8 bits</td>

   <td width="421">The value the CPU wants to Write. Will only have a meaningful value is CpuWrite = 1</td>

  </tr>

  <tr>

   <td width="118">LineFromMem</td>

   <td width="85">32 bits</td>

   <td width="421">The contents of the block you requested to read from memory</td>

  </tr>

 </tbody>

</table>




All of the inputs will only be valid during the <strong>SINGLE</strong> clock cycle that your Cache says it is Ready. This means that you will have to save your inputs if you need them beyond that first clock cycle.




The only <strong>exception</strong> to this is LineFromMem. It will be valid as long as you say you want to read from memory. If you aren’t reading from memory LineFromMem will be EEE… and Red (an Error). Don’t worry about this. It is normal behavior. As soon as your request to read from memory it will be the value at MemAddress.

<h1>Outputs</h1>




<table width="624">

 <tbody>

  <tr>

   <td width="105">Output Pin</td>

   <td width="98">Size (in bits)</td>

   <td width="421">Description</td>

  </tr>

  <tr>

   <td width="105">Ready</td>

   <td width="98">1 bit</td>

   <td width="421">1 if your Cache is ready to start a new request. If you are ready to start a new request it means you must have finished the old request. So at this point in time, your output will be checked.</td>

  </tr>

  <tr>

   <td width="105">DidContain</td>

   <td width="98">1 bit</td>

   <td width="421">1 if your Cache contained the value the CPU asked for and 0 otherwise. Checked when Ready == 1</td>

  </tr>

  <tr>

   <td width="105">ByteRead</td>

   <td width="98">8 bits</td>

   <td width="421">The value at the address the CPU requested to read. Checked when Ready == 1 and the last request the CPU made was a read.</td>

  </tr>

  <tr>

   <td width="105">MemAddress</td>

   <td width="98">8 bits</td>

   <td width="421">The address of the <strong>block</strong> you want to read from memory or the address of the <strong>block</strong> that you want to write Line2Memory back to.</td>

  </tr>

  <tr>

   <td width="105">MemRead</td>

   <td width="98">1 bit</td>

   <td width="421">1 if you want to read the <strong>block</strong> at MemAddress and 0 otherwise.</td>

  </tr>

  <tr>

   <td width="105">MemWrite</td>

   <td width="98">1 bit</td>

   <td width="421">1 if you want to write Line2Mem to MemAddress and 0 otherwise.</td>

  </tr>

  <tr>

   <td width="105">Line2Mem</td>

   <td width="98">32 bits</td>

   <td width="421">The <strong>block</strong> you want to write to MemAddress</td>

  </tr>

 </tbody>

</table>




<h2>Why is MemAddress 8 bits instead of 10?</h2>

Our Memory is going to be <strong>Block Addressable</strong> instead of Byte Addressable. This means that each block has an address instead of each byte. This enables you to read a block from/write a block to memory in a single clock cycle which should help simplify your design a little. It should be pretty easy to figure out which bits to drop to form MemAddress. Hint: it is bits that help you pick out the byte within a block.




<h2>When are outputs checked?</h2>

<ul>

 <li>DidContain: At the end of every operation once your circuit becomes <strong>Ready</strong>.</li>

 <li>ByteRead: At the end of a read operation once your circuit becomes <strong>Ready</strong>.</li>

 <li>MemRead: Whenever you read from memory it increments the MissCounter.</li>

</ul>

<h1>Timing Restrictions</h1>

Your circuit must be able to complete a Read/Write request <strong>within 10 clock cycles</strong>. If you take longer than 10 clock cycles the tester will automatically advance to the next case. This will cause your Cache to start to “desync” from the tester and so will get everything wrong if you are taking too long. You can certainly finish in less than 10 clock cycles, I only took 7 but I’ve seen students get as low as 2.




<h1>Testing</h1>

<ol>

 <li>Open the grading circuit</li>

 <li>Click on the Cache folder and select reload library

  <ol>

   <li>Double-check that your updates are visible inside of the grader. If your changes don’t show up, close your solution circuit and try reloading again. If the changes still fail to show up, close the grader and reopen it and then reload the library.</li>

  </ol></li>

 <li>Open the subcircuit named CheckerCirc and

  <ol>

   <li>Right-click the ROM inside of it</li>

   <li>Select Load Image</li>

   <li>Select one of the provided files that ends in _sol.txt.

    <ol>

     <li>Eample: seq_read1_seq_mem_sol.txt</li>

    </ol></li>

  </ol></li>

 <li>Open the subcircuit named InputGeneratorCirc and

  <ol>

   <li>Right-click the ROM inside of it</li>

   <li>Select Load Image</li>

   <li>Select the corresponding input file that matches with the solution file. This will be the file that shares its name with the first part of the solution file.

    <ol>

     <li>Example: seq_read1.txt</li>

    </ol></li>

  </ol></li>

 <li>Go back to the main subcircuit

  <ol>

   <li>Right-click the RAM</li>

   <li>Select Load Image</li>

   <li>Select the corresponding memory file that matches with the solution file. This will be the file that shares its name with the second part of the solution file.

    <ol>

     <li>Example: seq_mem.txt</li>

    </ol></li>

  </ol></li>

</ol>




Anytime your reset a test case you must <strong>RELOAD the RAM!</strong> The RAM is the only one that has to be reloaded. The ROM’s will maintain the last value you put in them.

At the bottom of the the main circuit you will find probes that show your circuit’s inputs, outputs, answers, how many reads you got correct, how many times you said you hit, and how many misses(memory reads) you had. This is a good place to look when you are debugging to get a high level overview of what your cache is doing.




<strong>If you open the solution files in a text editor you will find what the correct values for all of the stats should  be for that test case</strong>.

<h2>Testing Order</h2>

I recommend running the test cases in the following order

<ol>

 <li>seq_read1_seq_mem_sol.txt</li>

 <li>seq_write1_seq_mem_sol.txt</li>

 <li>set1_targeted_read_seq_mem_sol.txt</li>

 <li>set0_targeted_write_seq_mem_sol.txt</li>

 <li>random0_seq_mem_sol.txt</li>

 <li>final_test_rand_mem_mem_sol.txt.</li>

</ol>




<h2>User Created Tests</h2>

If you want to make your own test, you can use <a href="https://drive.google.com/file/d/1PhqvI-x4BJI-PF8n2knOmQrZpQQI3l0-/view?usp=sharing">cache_test_maker.py</a> to create your own tests. It should be fairly easy to follow the documentation inside if you feel like using it.