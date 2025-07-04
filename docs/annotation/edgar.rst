Comparative genomics with EDGAR
=================================

The EDGAR platform, a web server providing databases of precomputed orthology data for thousands of microbial genomes, is one of the most established tools in the field of comparative genomics and phylogenomics. Based on precomputed gene alignments, EDGAR allows quick identification of the differential gene content, i.e. the pan genome, the core genome, or singleton genes. Furthermore, EDGAR features a wide range of analyses and visualizations like Venn diagrams, synteny plots, phylogenetic trees, as well as Amino Acid Identity (AAI) and Average Nucleotide Identity (ANI) matrices. During the last few years, the average number of genomes analyzed in an EDGAR project increased by two orders of magnitude. To handle this massive increase, a completely new technical backend infrastructure for the EDGAR platform was designed and launched as EDGAR3.0. For the calculation of new EDGAR3.0 projects, we are now using a scalable Kubernetes cluster running in a cloud environment. A new storage infrastructure was developed using a file-based high-performance storage backend which ensures timely data handling and efficient access. The new data backend guarantees a memory efficient calculation of orthologs, and parallelization has led to drastically reduced processing times. Based on the advanced technical infrastructure new analysis features could be implemented including POCP and FastANI genomes similarity indices, UpSet intersecting set visualization, and circular genome plots. Also the public database section of EDGAR was largely updated and now offers access to 24,317 genomes in 749 free-to-use projects. In summary, EDGAR 3.0 provides a new, scalable infrastructure for comprehensive microbial comparative gene content analysis. The web server is accessible at http://edgar3.computational.bio.

References
^^^^^^^^^^

**EDGAR 3.0** https://doi.org/10.1093/nar/gkab341


