# TSBS Setup:
For the comparison of the two databases we utilized the [Time Series Benchmark Suite](https://github.com/timescale/tsbs) and, in particular, its 'Dev ops' use case, as it is the only "compatible" one with CrateDB. In the hyperlink above, one can find setup instructions for TSBS, which we also relied on, yet our exact, step-by-step, setup procedure is as follows:

For the successful installation of TSBS, we are, initially, required to have the Go Programming Language installed. To this end, we download (via wget) the necessary compressed file and unzip it (while also ensuring that Go isn't already installed in our system):
