In order to have reliable communication, channel coding is often employed. Turbo codes as a powerful coding technique have been widely studied and used in communication systems. Turbo coding is an advanced forward error correction algorithm. The decoding is the main step of channel codding and SOVA algorithm is one way to do that. In this project, two main goals of turbo code simulation with MATLAB and implementation on FPGA (VHDL Code) have been followed, which is summarized in four main sections with different subsections (functions and entities) as follows:
#	Simulation of turbo coding by MATLAB
1. Encoding (Main Function)
2.	Interlevear
3. RCS
#	Turbo decoding simulation by MATLAB
1.	Alfa
2.	Beta
3.	Decoder (Main Function)
4.	Deinterlevear2
5.	Demultiplex
6.	Gama_Sr_Ss2
7.	Interlevear_decoder
8.	M_alfa_beta_gama
9.	Match_data_parity
10.	Probablity_bit_data
11.	Trelis2
#	Describe and test the turbo coding by VHDL
1.	Encoder (TOP LEVEL)
2.	freq_divider3t1
3.	interleaver
4.	mux3t1
5.	mux8t1
6.	RSC
7.	shift_reg
8.	tb_encoder
9.	tb_freq_divider3t1
10.	tb_interleaver
11.	tb_mux3t1
12.	tb_mux8t1
13.	tb_RSC
14.	tb_shift_reg
#	Describe and test the turbo decoding by VHDL
1.	abPermut
2.	accDist
3.	accDistSel
4.	acs
5.	adder
6.	clkDiv
7.	clkrst
8.	cmp2
9.	cod2
10.	cod3
11.	coder
12.	delayer
13.	distance
14.	distances
15.	extInf
16.	interleaver
17.	iteration
18.	limiter
19.	min4
20.	min8
21.	mux2
22.	mux4
23.	mux8
24.	opposite
25.	partDistance
26.	punct
27.	reduction
28.	reg
29.	sova (TOP LEVEL)
30.	stateSel
31.	subs
32.	tb_sova
33.	tb_turboDec
34.	trellis1
35.	trellis2
36.	turboDec (TOP LEVEL)
37.	turbopack
38.	zPermut
# System and Application requirements
## The application requirements for using Turbo-Code are
1.	MATLAB version (2022b) or later.
2.	Xilinx ISE 14.7.
# Running the Matlab turbo code simulation
1.	Download Matlab Encoder and Decoder Files.
2.	Run Matlab.
3.	Go to project.
4.	Run Main Functions (Encoding and Decoder).
5.	Give it your input vector.
# Running the VHDL turbo code implementation and simulation
1.	Download VHDL Encoder and Decoder Files.
2.	Run ISE.
3.	Go to project Directory.
4.	TOP LEVEL ENTITIES are Encode, Sova, and turboDec, respectively.
5.	Simulate tb_encode, tb_sova, and tb_turboDec, respectively.
# More details about turbo code
The discovery of turbo codes was a monumental event in the error control coding discipline, one that fundamentally changed the way it will be thought of and practiced. In many respects the introduction of turbo codes in 1993 was a turning point in the field of error control coding, when, after more than 50 years of striving to achieve Shannon’s capacity limit, it was suddenly and unexpectedly achieved. Although LDPC codes had been introduced many years earlier, their potential had not been realized, and it was only after Berrou, Glavieux and Thitimajshima presented “Near Shannon limit error correcting and decoding: turbo codes” that the combination of concatenation, random-like interleaving and iterative decoding became the new standard for capacity-approaching error correction. The near-capacity performance of turbo codes and their novel iterative decoding algorithm has stimulated an explosion of research efforts to understand this new coding scheme.
The choice of component codes appears to have reached a consensus. There is only a finite number of primitive polynomials of a given degree and computer searches have narrowed down the set of possible numerator polynomials to those with the best effective free distance. Turbo applications use IIR convolutional codes with v = 3memory elements (such as in wireless telephony) or v = 4 memory elements (such as in space applications). When designing a turbo code, the choice comes down to a trade-off between extra complexity (eight versus 16 states) and around a dB of performance improvement.
The design of turbo interleavers, however, is still under discussion. The good BER performances of the S-random interleavers introduced in and their extensions (see e.g.) have made them a de facto standard for comparison with new interleaver proposals. The interleavers used in practice are tweaked, usually by trial and error, to fit the particular component codes being employed. New, promising, algorithmic interleavers are based on permutation polynomials that can be algebraically tailored to the given component codes. The exceptional performance of turbo codes is due to the long pseudo-random interleaver, introduced below, which produces codes reminiscent of Shannon’s noisy channel coding theorem, and to the low-complexity iterative algorithm that makes their implementation feasible.
The choice of the interleaver is crucial in the code design. Interleaver is used to scramble bits before being input to the second encoder. This makes the output of one encoder different from the other encoder. Thus, even if one of the encoders occasionally produces a low-weight, the probability of both the encoders producing a low-weight output is extremely small. This improvement is known as interleaver gain. Another purpose of interleaving is to make the outputs of the two encoders uncorrelated from each other. Thus, the exchange of information between the two decoders while decoding yields more reliability. There are different types of interleavers, e.g., row column interleaver, helical interleaver, odd-even interleaver, etc. For parallel concatenation codes (turbo codes), the blocks are connected in parallel. The parallel concatenation scheme is shown in Fig. 1.
 ![1 jpg](https://user-images.githubusercontent.com/98907330/214027658-379ec659-ed18-4502-8071-eaf660db34bd.jpeg)
Fig. 1. Parallel concatenated code.

# Encoder
The generic form of a turbo encoder consists of two encoders separated by the interleaver. The two encoders used are normally identical and the code is systematic, i.e., the output contains the input bits as well. Turbo codes are linear codes. Linear codes are codes for which the modulo sum of two valid code words is also a valid codeword. A “good” linear code is one that has mostly high-weight code words. The weight or Hamming weight of a codeword is the number of ones that it contains, e.g., the Hamming weight of codeword ‘000’ and ‘101’ is 0 and 2 respectively. High-weight code words are desirable because it means that they are more distinct, and thus the decoder will have an easier time distinguishing among them. While a few low-weight code words can be tolerated, the relative frequency of their occurrence should be minimized. To summarize, it can be said that turbo codes make use of three simple ideas: 
1.	Parallel concatenation of codes to allow simpler decoding 
2.	Interleaving to provide better weight distribution 
3.	Soft decoding to enhance decoder decisions and maximize the gain from decoder interaction. 

The fundamental turbo code encoder is built using two identical recursive systematic convolutional (RSC) encoders concatenated in parallel. Convolutional codes are usually described using two parameters: the code rate r and the constraint length K. The code rate k/n, is expressed as a ratio of the number of bits into the convolutional encoder k to the number of channel symbols output by the convolutional encoder n in a given encoder cycle. The constraint length parameter K denotes the length of the convolutional encoder, i.e., the maximum number of input bits that either output can depend on. Constraint length K is given as k = m + 1. Where m is the maximum number of stages (memory size) in any shift register. The shift registers store the state information of the convolutional encoder and the constraint length relates the number of bits upon which the output depends.
In turbo code encoder, both the RSC encoders are of short constraint length in order to avoid excessive decoding complexity. An RSC encoder is typically of rate r = 1/2 and is termed a component encoder. The two component encoders are separated by an interleaver. The output of the turbo encoder consists of the systematic input data and the parity outputs from two constituent RSC encoders. The systematic outputs from the two RSC encoders are not needed because they are identical to each other (although ordered differently) and to the turbo code input.
The first RSC encoder outputs the systematic output c1 and recursive convolutional encoded output sequence c2 whereas the second RSC encoder discards its systematic sequence and only outputs the recursive convolutional encoded sequence c3.
As previously mentioned, at the encoder, turbo codes use a parallel concatenation of two convolutional component encoders, as shown in Fig. 2. The length-K message u is encoded directly by the first component encoder, which produces the parity bits p(1); however, it is interleaved (see below) before being encoded by the second convolutional encoder, which produces the parity bits p(2). Turbo codes are systematic codes; thus the bits in u, p(1) and p(2) are all transmitted.
 ![2 jpg](https://user-images.githubusercontent.com/98907330/214027962-7876d6ad-65fa-4afa-a2bb-c9966b6a6446.jpeg)
Fig. 2. Block diagram of a turbo encoder system.

Since both encoders, encode the same message bits, their decoders can cooperate with one another at the receiver to decode the message. Importantly, the set of message bits is permuted, or interleaved, before reaching the second convolutional encoder. Using interleaving, the parity bits output by the two encoders will be different even if both encoders are the same.
Turbo codes use an interleaver between two component encoders. The purpose of using the interleaver is to provide randomness to the input sequences and increase the weight of the code words. A number of interleavers are used in turbo codes.
# Decoder
The traditional approach to decoding convolutional codes is by using the Viterbi Algorithm (VA) that provides a Maximum Likelihood (ML) sequence. The VA in the most general form is a maximum a-posteriori probability sequence estimator that finds the maximum likelihood path through the trellis diagram given a specific received sequence. Mathematically, the VA finds the rellis path that maximizes the a-posteriori probability P(S|Y ). The Soft-Output Viterbi Algorithm (SOVA) decoder outputs soft values that makes it suitable for use in turbo decoders. The modifications to enable the traditional hard-decision VA to give soft-output values was proposed in. Fig. 3. shows block diagram of turbo-decoder with SOVA method.
![3 jpg](https://user-images.githubusercontent.com/98907330/214042018-1ae47dcc-b21d-4fed-a777-5922288a38e1.jpeg)
fig. 3. Block diagram of turbo decoder. 

