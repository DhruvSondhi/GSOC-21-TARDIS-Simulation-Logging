# GSOC'21 @ TARDIS : Improving TARDIS Simulation Logging Framework

![GSOC Banner](Images/GSOC_banner.png)

As all good things always come to an end, Google Summer of Code 2021 is no such exception. Learning & Working with Open Source Organisations is an awesome endeavour that all people must try once. I am very happy that I was selected in TARDIS for my GSOC project on Improving TARDIS Simulation Logging Framework. I would like to report my work in this Repository.


## About TARDIS
![TARDIS](Images/TARDIS_Banner.svg)

[**TARDIS**](https://tardis-sn.github.io/tardis/index.html) {Temperature And Radiative Diffusion In Supernovae} is an Open Source Astophysics Organisation that participated under GSOC'21. It provides Python packages for astronomy research for analysing supernovae. It uses the Monte Carlo Radiative-Transfer spectral synthesis code for 1D models for supernova ejecta. TARDIS can accurately model the supernova ejecta and allows for the analysis along with tools for Visual Representation of the data by providing custom Widgets for plots of different parameters. It makes the analysis very user friendly & easy to comprehend. 

Logging & Tracking are a big part of this project as spectrum generation requires analysis of particle properties which are presented via this functionality. Logs are present whenever we run TARDIS simulations allowing for easy logging of the simulation status and validation of the results thus generated. Tracking can de done on need basis with the type of analysis.

## About the Project 
![tardis gsoc project](Images/TARDIS_GSOC_Project.png)
My project this summer with TARDIS was Improving TARDIS Simulation Logging Framework. This project deals with setting up and configuring the Simulation Logging such that we are able log & debug long TARDIS simulation runs. The logging system needed to be reconfigured & implemented such that it could be configured on need bases. 

Another aspect of my project was to log NUMBA JIT'ted functions such that input & output values for a particular function could be logged correctly. This is not possible as NUMBA based functions do not allow traditional logging to be implemented due to the way the computation happens.

Tracking was another key part of my project. RPacket Tracking was implemented which allows the tracking of all the packet interactions for all the packets in all the iterations. This functionality allows the user to have a Dataframe that stores all these values and hence computation & analysis can be done on the same. Visualization can also be done with the data thus obtained.


## Work Product
The deliverables of my project were as follows:

- [x] Improve & Implement a Logging Framework for the Simulations runs in Notebooks as well as terminals [[PR #1633](https://github.com/tardis-sn/tardis/pull/1633)]
- [x] Allow the Logging Framework for the simulation to be configured by functional arguments passed when invoking as well as from YAML config file functional arguments [[PR #1633](https://github.com/tardis-sn/tardis/pull/1633)]
- [x] Adding functionality to detect the environment where TARDIS simulation is running [[PR #1650](https://github.com/tardis-sn/tardis/pull/1650)]
- [x] Create a new Ipython Widget that can be used to store & display Plasma Values when logging is turned off [[PR #1640](https://github.com/tardis-sn/tardis/pull/1640)]
- [x] Create a new Logging module under `tardis/io` to keep all the tracking & logging based functionality in a single place [[PR #1684](https://github.com/tardis-sn/tardis/pull/1684)]
- [x] Create a Flow chart which maps the flow of the whole TARDIS architecture when running a Simulation 
- [x] Add logging messages for caught exceptions in Simulation runs [[PR #1701](https://github.com/tardis-sn/tardis/pull/1701)]
- [x] Add debug messages for status of different parameters in logs for Simulation runs [[PR #1704](https://github.com/tardis-sn/tardis/pull/1704)]
- [x] Implement a Tracking Framework for RPackets such that interactions & properties for the packets can be stored & analysed [[PR #1748](https://github.com/tardis-sn/tardis/pull/1748)]
- [x] Storing the interaction properties of the Packets in a DataFrame [[PR #1776](https://github.com/tardis-sn/tardis/pull/1776)]
- [x] Various Tests implemented to for all the functionalities
- [x] Documentation & Tutorials added for the same

### Implementing & Improving Logging Framework
The logging framework present initially was not upto the mark with low number of debug messages & didn't have the functionality to be configured by the user. In my project, I improved & improvised on the information that is being presented while the simulation is run. Functionality was added which allows the user to configure the logging framework based on their specific needs. These include setting up the logger to a particular `log_level` or specifying `specific_log_level` such that log messages of that particular level be displayed.<br>
This implementation allows configuring the `log_level` as well as `specific_log_level` values from the functional arguments such as `run_tardis(config, log_level="info", specific_log_level=True)` or via the TARDIS config `YAML` as follows: 
```yaml
...
debug:
    log_level : "Debug"
    specific_log_level : True
```
To learn more about this functionality please refer to the TARDIS documentation on [`Configuring the TARDIS Logger`](https://tardis-sn.github.io/tardis/io/optional/logging_configuration.html)

_enabling logging functionality via Functional arguments being passed to `run_tardis()` function_
![logging](Images/normal_run.png)
_configuring the logging functionality from the `TARDIS YAML` config_
![Yaml_logging](Images/Yaml.png)

### Adding Functionality to detect run environment of TARDIS simulation
This functionality was a need as the log messages were formatted such that pandas Dataframe could be displayed inside the Jupyter & Jupyter based environment allow for better formatting & displaying a printed table when run outside the IPython kernel or terminals. This allows TARDIS to have differnt logging output formatting for differnt environments as can be seen below:
_sample simulation run inside Jupyter environment_
![jupyter_sim](Images/normal_run.png)
_same simulation run inside a terminal (non jupyter based env)_
![terminal_sim](Images/terminal1.png)

### Flow chart for TARDIS Simulation
A flow chart was created which accurately maps the flow of data & how the simulation is being created & run from start to finish including values being read from the TARDIS Configuartion YAML. The Flow chart can be seen below:
![tardis_flow_chart](Images/Simulation_Logging_Flow_Diagram.jpeg)

### Plasma State Widget
`Plasma State Widget` is a work in progress feature that is being added to TARDIS Simulation (as of writing this blog). This Widget's main functionality is to allow for analysis of the Radiative Temperature `t_rad` and Dilution Factor `w` for each iteration of the TARDIS simulation. This allows to access these values when the simulation logging has been turned off. A snippet of the same can be seen below:
_output of the plasma widget once the simulation run has been completed_
![plasma_state_widget](Images/plasma_state_widget.png)

### Adding Debug messages for Caught Exceptions
Caught exceptions are those exceptions that are handled via Exceptions handling, i.e. in simpler terms `try ... except` exceptions. These exceptions were not logged & hence status about different errors present while setting up the parameters for the simulation was not available. Adding Debug Log messages to track these changes allow for debugging state of the simulation.

### Adding More Debug Messages to the Simulation
Multiple debug message (more than 30) were added to TARDIS simulation loggers. These messages now allow for meaningful information to be present in the simulation & analysed or debugged when a simulation has run for a set of input parameters in the `YAML Config`.
_sample debug messages that have been added to tardis_
![debug_message](Images/debug_msg.png)

![debug_messages2](Images/debug_msg2.png)

### Tracking RPacket Interactions 
This is one of the main proposed idea of my project. Tracking RPacket interaction is a tedious task as it needs to be configured such that it works with NUMBA. NUMBA doesn't have the support for native logging to be present, though the usage of `with objmode` context manager, leads to performance loss. Thus, this packet tracker can be configured via the YAML config. 
The packet Tracker (know as `rpacket_tracker` ;)) allows to track & store all the interations of all the packets that are generated when the simulation is run in each iteration. A `DataFrame` is present to store all these values allowing easy analysis on the data with `pandas`.
The functionality of the same is show as follows:
![interaction_dataframe](Images/df_interaction.png)

For more information, please do checkout the TARDIS Documentation for `Tracking RPacket Interaction`. _(Work in progress)_

### Tests & Documentation
Before my coding period started, I was contributing to improve the Documentation for TARDIS. [***Issac Smith***](https://github.com/smithis7) has improved the documentation many folds with imporovements in various areas for explaining difficult physics concepts in simple yet elegant way. 
Tests as well as Documentation (Tutorials) were added to TARDIS for each of the aforementioned functionality. Test driven development was a key while planning the different feature as TARDIS needs to be precise & well tested for all the analysis that can be done.

## Contributions Done :
The following table shows some of the highlights of the contributions that I have done in GSOC'21 with TARDIS:
| PR Title                                            | PR Number                                             | Status           |
| --------------------------------------------------- | ----------------------------------------------------- | ---------------- |
| Formatting Logging Output                           | [1632](https://github.com/tardis-sn/tardis/pull/1632) | Merged           |
| Implementing Logging Framework                      | [1633](https://github.com/tardis-sn/tardis/pull/1633) | Merged           |
| Adding Plasma State Widget                          | [1640](https://github.com/tardis-sn/tardis/pull/1640) | Draft            |
| Functionality to detect Run Environment             | [1650](https://github.com/tardis-sn/tardis/pull/1650) | Merged           |
| Restructured Logging framework to `tardis/io`       | [1684](https://github.com/tardis-sn/tardis/pull/1684) | Merged           |
| Added logging support for caught exceptions         | [1701](https://github.com/tardis-sn/tardis/pull/1701) | Merged           |
| Added Debug Messages to Simulation Logs             | [1704](https://github.com/tardis-sn/tardis/pull/1704) | Awaiting Review  |
| Changed Formatting based on log level               | [1710](https://github.com/tardis-sn/tardis/pull/1710) | Draft            |
| Renamed `montecarlo_logger` to `montecarlo_tracker` | [1740](https://github.com/tardis-sn/tardis/pull/1740) | Merged           |
| Tracking RPacket Properties in `single_packet_loop` | [1748](https://github.com/tardis-sn/tardis/pull/1748) | Work In Progress |
| Adding DataFrame for RPacket Tracking               | [1776](https://github.com/tardis-sn/tardis/pull/1776) | Work In Progress |

I have also made serveral PRs to bug fix some of the bugs that were encountered in the project. Tests & Documentation were a part of some of the aforementioned PRs. I have also created some issues which can be seen [here](https://github.com/issues?q=is%3Aopen+is%3Aissue+author%3ADhruvSondhi+archived%3Afalse+org%3Atardis-sn). I also contributed to the [`TARDIS Website`](https://github.com/tardis-sn/tardis-sn.github.io/) and the [`Documentation`](https://tardis-sn.github.io/tardis/index.html) of TARDIS before the GSOC Coding period started. These contributions can be seen [here](https://github.com/tardis-sn/tardis-sn.github.io/pulls?q=is%3Apr+author%3A%40me+) ([Website](https://github.com/tardis-sn/tardis-sn.github.io/)) as well as [here](https://github.com/pulls?q=author%3ADhruvSondhi+archived%3Afalse+org%3Atardis-sn+created%3A%3C2021-06-07) ([Documentation](https://tardis-sn.github.io/tardis/index.html))

All my contributions can be seen on this [link](https://github.com/pulls?q=author%3ADhruvSondhi+archived%3Afalse+org%3Atardis-sn).

## Future Scope
Some of my proposed ideas were not implemented in the given time frame for GSOC'21. These include:

- **NUMBA Logger** : Implementation of a Logging framework for logging the Input & Output of the JIT'ted functions. Additional functionality include logging of the list of arguments being passed into the function, time taken to run the function, etc.
- **Real Time Logging** : This functionality, when implemented, would allow for tracking & analysing packet properties after the simulation has run. It could allow to infer packet properties based on time frame selected in an iteration.
- **Allow `single_packet_loop` to be accessible without setting up the Simulation** : This functionality will allow a single packet to be passed directly inside the `single_packet_loop` for analysis of the interactions and its properties. 


## Conclusion
My project did have some deliverables changed but implementation went smooth. I learnt a lot of new stuff & working directly with the internals of the TARDIS, was tricky but it gave me lot of knowledge about how TARDIS actually works. My project was not completed in the given time frame, but I will continue contributing to TARDIS in the near future & implement all the remaining features. This will allow for TARDIS to have a better Logging functionality.

## Acknowledgement 
I would like to thank [**Andrew Fullard**](https://github.com/andrewfullard), [**Jack O'Brien**](https://github.com/Rodot-) & [**Wolfgang Kerzendorf**](https://github.com/wkerzendorf) for all the guidance & help they have provided me throughout this endeavour. They have helped me learn a lot of new stuff in Python as well as help me understand how TARDIS works & how different features are implemented. They have given me alternative ways to implement difficult problems with simpler solutions leading to learning new concepts.

![Gather_town](Images/Gathertown.png)

I would like to thank ***TARDIS Collaboration*** for giving me the opportunity to work here & learn about the awesome research that is done with TARDIS. I would like to specially thanks **Andrew** & **Wolfgang** for encouraging me & supporting me from the start (before the selection for GSOC) and helping me learn & grow.
