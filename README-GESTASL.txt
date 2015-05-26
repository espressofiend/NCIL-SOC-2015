The file GESTASL-s001-Run1_log.csv is a log file from an fMRI experiment that was
run in DirectRT. In the experiment, we contrasted brain activation when viewing
gestures with brain activation when viewing control stimuli (3 videos overlaid
and played backward). On each trial, the subject saw a cue which informed them
whether it was a gesture trial (in which case they were to perform a comprehension
task) or a control trial (in which case they were to perform a control task).
One second after the cue appeared, the gesture or control movie would play. These
had variable durations. Following the movie, there was a blank screen of varying
duration and then a prompt to make a response.

The following is an explanation of the relevant columns; open the CSV file in
Excel to view the data:
Column L/RT: reaction time in millseconds - time between onset of response cue and subject's response
M/Correct: TRUE if correct answeerw was given, else FALSE
N/Started@: time the trial started at, relative to when the stim program was started, in msec
O/Load time: Amount of time, after the trial started, required to load the stimuli. But see next column...
P/TotalInt: Load time + amount of extra time it took before the first stimulus of the trial (which was a blank screen) to be shown on-screen. This accounts for the fact that after the load time, DirectRT needs to wait for the next screen refresh to draw a stimulus, which can be as much as 17 msec.
Q/Stim 1: First stimulus in the trial. A blank screen.
R/Time 1: Duration of first stimulus
S/Stim 2: the cue stimulus
T/Time 2: duration of the cue
U/Stim 3: the identity of the movie file shown. Includes extra information we don't need, that we'll need to filter in our script
V/Time 3: Duration of the movie
W/Stim 4: blank screen after the movie
X/Time 4: duration of blank screen (random interval)
Y/Stim 5: Response prompt shown on L side of screen
Z/TIme 5: Duration before next stim was drawn; zero because the R response prompt was drawn immediately after (effeectively simultaneously)
AA/Stim 6: Response prompt shown on R side of screen
AB/Time 6: Duration of response prompt display before trial ended. Basically equivalent to RT but a bit longer.

What we want from GESTASL-s001-Run1_log.csv is three output files: one for
behavioural analysis and two for fMRI analysis. For fMRI, we need one file with
the timing of the Gestures stimuli, and a second file with the timing of the
control stimuli.

Contents of behavioural output file:
Trial Num | Condition [gest/ctrl] | RT | Correct/Incorrect

Contents of fMRI output files:
Onset of movie relative to MRI start time (msec) | Duration of Movie (msec) | 1
...the third column is literally the number '1' on each line. This is a requirement
of the FSL analysis package, as is having a separate timing file for each condition.

A note about timing: Time 0 is when the experimenter started the stim program,
however after the program was started, it displayed instructions to the subject
then waited for a "trigger" from the MRI indicating that the scan had started and
the stimulus presentation should begin. Once the trigger was received, the scan
started with a ~8 sec fixation cross before the first trial. Practically speaking,
what this means is that the "time zero" that the stimuli in the scan should be
referenced to in the output file you create should not be relative to "0" in the
log file, but instead relative to the "Started@" time for the fixation cross,
because that's when the MRI scan started so that's when our first data point was
recorded. Therefore to get the true onset time of each trial, we need to subtract
the "Started@" time for the fixation cross from the "Started@" time for every
subsequent trial.
