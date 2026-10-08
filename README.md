# TRIK PID Line-Following Project

## Project file

Open `240045_improved.qrs` in TRIK Studio. It is based on the original `240045.qrs`; the original project has not been changed. The improved project uses clearer variable names in its initialization, PID calculation, and motor-power expressions.

## Variables

### Sensor reference values

- `left_sensor_reading` — initial/reference reading from `sensorA4`.
- `right_sensor_reading` — initial/reference reading from `sensorA3`.

### PID settings and state

- `proportional_gain` (`1.3`) — scales the current error.
- `integral_gain` (`0.02`) — scales the accumulated error.
- `derivative_gain` (`0.02`) — scales the rate of change of error.
- `time_step` (`0.01`) — time interval used by the integral and derivative calculations.
- `integral_accumulator` (`0` initially) — running sum of error over time.
- `integral_limit` (`60`) — limits the accumulator to reduce integral windup.
- `previous_error` (`0` initially) — error from the previous control iteration.

### Values calculated each iteration

- `error` — difference between the current sensor readings and their reference readings:
  `(sensorA4 - left_sensor_reading) - (sensorA3 - right_sensor_reading)`.
- `proportional_term` — `proportional_gain * error`.
- `integral_term` — `integral_gain * integral_accumulator`.
- `derivative_term` — `derivative_gain * (error - previous_error) / time_step`.
- `control_output` — sum of the P, I, and D terms; used as the steering correction.

## Control flow

1. Initialize the sensor reference values, PID gains, time step, and stored state.
2. Calculate the current sensor error.
3. Update and limit the integral accumulator.
4. Calculate the proportional, integral, and derivative terms.
5. Sum the terms into `control_output` and save the current error as `previous_error` for the next iteration.
6. The motor block uses `40 + control_output` as its power expression. The other motor expression remains as it was in the original project.

## Notes

- Only variable names and the corresponding motor expression reference were changed; the PID constants and control formulas were retained.
- The names `sensorA3` and `sensorA4` are TRIK sensor identifiers and were not renamed.
- This README describes the project file; confirm behavior by opening and running it in TRIK Studio with the intended robot or simulator.
