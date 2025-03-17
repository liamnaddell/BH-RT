----

Dear Marcelo Ponce,

Thank you again for your submission to PEARC25. We received many high quality submissions this year and we regret to inform you that your submission "Parallel Ray Tracing of Black Hole Images Using the Schwarzschild Metric" has not been accepted for presentation at PEARC25.

Reviewers' feedback is provided below. You may want to resubmit your work as a poster/vizualization or short paper, which have respective submission deadlines of 11:59 PM ET March 22 and March 29. For any resubmission, be sure to take the reviewers comments into account and to adhere to the new format requirements (refer to the submission guidelines:    https://pearc.acm.org/pearc25/call-for-participation-submission-guidelines/ ) 

We hope you will join us for a great opportunity to connect with other research computing enthusiasts! You can register at  https://pearc.acm.org/pearc25/registration/

Regards,
Jason Yalim <yalim@asu.edu> and Karen Tomko <ktomko@osc.edu>
PEARC25 Applications and Software Track Co-chairs

========================================================================

SUBMISSION: 74
TITLE: Parallel Ray Tracing of Black Hole Images Using the Schwarzschild Metric


----------------------- REVIEW 1 ---------------------
SUBMISSION: 74
TITLE: Parallel Ray Tracing of Black Hole Images Using the Schwarzschild Metric
AUTHORS: Naddell Liam and Marcelo Ponce

----------- Overall evaluation -----------
SCORE: 1 (1: weak accept)
----- TEXT:
This is a reasonably well-written paper as the math theory is sound and the techniques chosen for parallelization is proper. The authors were able to tune different hyper-parameters of the ray tracing problem and the number of OpenMP threads/MPI processes involved, which thoroughly evaluated the effectiveness of the parallel implementation. For pedagogical purposes, the paper presents a good example of using HPC resources to speed up the solution to real-world problems.

I recommend the authors to check the following places to make improvements:
(1) Mathematically, it is a little confusing to use the terminology "2D spherical coordinates." Under 2D, we typically say "polar coordinates."
(2) In 2.6, the discussion on how to decompose the original domain can be enhanced. Have the authors considered interleaving the scanlines (rows) among threads (processes)? If so, typically, the original domain will be almost evenly distributed among parallel units, and the mentioned large remainder chunk will not be a problem.
(3) It appears that figure 6 and 7 are based on the results from a single HPC node, the paragraph on the next page does not clarify this.
(4) Since the four figures 4~7 are all based on a single HPC node, the x-axis title should all be "cores", correct? The authors used "cores" for Figure 4 and 5, but "nodes" for 6 and 7, which causes a little confusion to readers.
(5) The 2nd paragraph explaining Figure 4 and 5 needs re-work. As the authors increase the number of cores and the width in tandem, the overall workload actually increases quadratically. Although the number of cores increase linearly, each core is actually getting more work. This is why the time in both figures increases. The time curve obviously wiggles around the straight line that increases the width, this is well-explained by the fact that "quadratically-increased-overall-workload / linearly-increased-cores = linearly-increased-workload-per-core". The authors' claim about the ideal case, in which they expect to see consistent time that does not increase, seems to be wrong.



----------------------- REVIEW 2 ---------------------
SUBMISSION: 74
TITLE: Parallel Ray Tracing of Black Hole Images Using the Schwarzschild Metric
AUTHORS: Naddell Liam and Marcelo Ponce

----------- Overall evaluation -----------
SCORE: 3 (3: strong accept)
----- TEXT:
Parallel Ray Tracing of Black Hole Images using the Schwarzchild metric
This paper presents an innovative approach to generating images of black holes using parallel ray tracing techniques in conjunction with the Schwarzschild metric. The authors have developed a high-performance computing method to simulate and visualize the complex gravitational lensing effects caused by black holes. By implementing a parallel algorithm, the authors’ goal is to provide an open-source, minimalistic methodology and hardware-agnostic approach. This allows for the implementation to be accessible to students in physics and computer science education. This provides practical physics concepts with computing techniques to solve complex scientific problems using images of black holes.
        The paper's approach using open-source, minimalistic methodology is a significant strength, as it promotes transparency and reproducibility in scientific research. This approach allows other researchers and students to easily access, understand, and build upon the work. The hardware-agnostic implementation is another notable feature, ensuring broad accessibility and applicability across different computational environments. This is particularly valuable in educational settings where resources may vary. One possible improvement to the paper could involve evaluating the results. How are the results evaluated, by eye, or are there other ways that can determine that the desired results were achieved? How do people know the results are acceptable? (understanding that this is not the goal of the paper  but worthy of mention) In conclusion, the paper’s process to create these images, the computational needs to do so, and the explanations make this a good contribution to the confe
rence. Additionally, paper is well written with no grammatical errors or suggested changes in that regard.



----------------------- REVIEW 3 ---------------------
SUBMISSION: 74
TITLE: Parallel Ray Tracing of Black Hole Images Using the Schwarzschild Metric
AUTHORS: Naddell Liam and Marcelo Ponce

----------- Overall evaluation -----------
SCORE: -2 (-2: reject)
----- TEXT:
This paper describes the implementation of a parallel open-source program that can ray trace images in the presence of a black hole geometry. As noted by the authors, there are already a large variety of ray tracing implementations, including some used for direct astrophysical applications such as the rendering of black hole surroundings. However, the authors assert that their approach is set apart by its inclusion of several specific features:

1. It adopts a “minimalistic and simplistic implementation” strategy that deals mainly with the features of the actual mathematical problem, while relying on specialized tools and libraries (such as the Gnu Scientific Library) to address technical computations such as solving the governing ordinary differential equation (ODE).

2. It achieves reasonable levels of efficiency and performance by employing standard software tools (e.g., OpenMP and MPI) to implement parallel versions.

3. It is hardware agnostic and avoids dependence on specialized hardware, GPUs, etc.

Strengths:

1. The authors came up with creative solutions to several significant implementation problems, such as memory constraints and using the governing ODE itself to map light rays between 3D and 2D-spherical domains.

2. The authors provided useful discussion on several topics. The research originated from a project in an advanced computing class, so there is significant discussion of the educational value to students who would implement this type of ray tracer.

Weaknesses and Additional Comments:

While this paper may be of interest to some PEARC attendees, particularly those who are looking for applications that might be taught in the classroom, I believe there are too many weaknesses to recommend acceptance. Among these are:

1. I did not feel that the work was sufficiently novel to warrant acceptance for PEARC. As noted above, the authors listed four differentiating features of their research, but I did not find these to be compelling. Moreover, in a current-day research paper focused mainly on parallel performance, I would think that more consideration must be given to GPUs or other parallel accelerators.

2. I found the paper difficult to read and understand. Admittedly, I am not an expert in ray tracing, but I know a fair bit about parallel computing and numerical computation, so I should have been able to follow the details. As just one example, it was hard to follow the discussion related to Fig. 1, partly because the mapping between features of the figure and the description in the main text was not clear.

3. In the discussion of domain decomposition, the paper describes how to balance the assignment of scanline rows to each MPI process. However, it’s not obvious that this necessarily means that the computational load is balanced, since the computational loads for the pixels will depend on the numerical method used for the ODE (which is not specified, so far as I can see---GSL contains several different ODE solvers) and, potentially, on the details of the particular MPI implementation available on the Niagara supercomputer. (The latter may be relevant because most runs were made on a single node, so that the MPI runtime system might well communicate via local shared memory in order to avoid using network communication.)

4. The paper includes several pages that focus on parallel performance of the ray tracer. I have a number of concerns in this area:

  a. The paper is not clear about which CPU was used for the performance plots, listing both SkyLake and CascadeLake. There may be significant performance differences between those two processor families, so it is important to be clear which type was used (and to make sure that all runs used the same type of core). It is also important to know whether the Intel TurboBoost functionality was on or off.

  b. To generate the weak scaling plots, the authors adjust, in tandem, the values of two parameters (e.g., simultaneously increasing the number of OpenMP threads and the width of the image in Fig. 5). However, the authors do not indicate what step-sizes were used to make the adjustments, making it difficult to evaluate the meaning of the plots. This omission could be particularly important for the plot in Fig. 8, where the authors themselves are uncertain what causes the behavior seen in the plot.

  c. As noted by the authors, the epsilon parameter seems particularly interesting to study, since changing its value could impact both the render quality and the parallel performance. Among other things, changing epsilon (as in Fig. 8) potentially impacts the time spent solving the ODE. It is unfortunate that the authors did not focus more on this type of scaling analysis.



----------------------- REVIEW 4 ---------------------
SUBMISSION: 74
TITLE: Parallel Ray Tracing of Black Hole Images Using the Schwarzschild Metric
AUTHORS: Naddell Liam and Marcelo Ponce

----------- Overall evaluation -----------
SCORE: 0 (0: borderline paper)
----- TEXT:
This paper describes an open-source ray-tracing application, employing a standard scientific library (GSL) and both shared and distributed parallelization, intended for black hole images. The application's function and motivation are well presented. After presentation of the theoretical underpinnings, the authors describe the computational implementation including discretization, extension to 3D, data size considerations, domain decomposition and parallelization. Two figures present the spectacular image outputs from this tool. Performance analysis, including strong and weak scaling, as well as some analysis of the impact of the discretization, is followed by some discussion on the anticipated impact of this tool with emphasis on educational use.

The authors provide a repo for this open source code, and stress its non-use of accelerators (such as GPU, which is well-known for ray-tracing) for wider applicability. Larger image considerations and future performance tuning are included. The topic and presentation of this work aligns well with the PEARC conference.

The scaling analysis section needs some additional information and important edits. First, EITHER the speedup axis on strong scaling plots 6, 7 and 9 must be adjusted to indicate that the speedup is not perfect "45 degree" by matching to the x-axis scale OR an "ideal line" for perfect strong scaling must be added. Second, the weak scaling plots in figures 4 and 5 are very noisy. Were repeated measurements taken for each datapoint in these plots? It is standard practice to take multiple measurements and plot a summary statistic. This measurement process must be described in the text.


-----
