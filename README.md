AKILES 2D
=========

Matlab implementation of the kinetic plasma plume model described in
M. Merino et al., "Direct-Vlasov Study of Electron Cooling Mechanisms
in Paraxial, Unmagnetized Plasma Thruster Plumes," IEPC-2017-104.
The name Akiles2d stands for "Advanced Kinetic 
Iterative pLasma Expansion Solver 2D."

The first version of this code has been funded by ESA under contract 
4000116180/15/NL/PS. It has been developed by Mario Merino and Javier
Mauriño, the latter during a research visit to the [EP2](http:\\ep2.uc3m.es)
group at UC3M, funded by a UK Royal Academy of Engineering Engineering Leaders
Scholarship (ELAA1516/1/87). 

Akiles2d solves iteratively for the electric potential and the EVDF weight in
a paraxial, unmagnetized, steady-state plasma plume to satisfy quasineutrality
and a global condition on the net electric current. It can be readily used for
magnetized plumes in magnetic nozzles as well. After successful convergence to
the self-consistent solution, the Akiles2d computes the most frequently used
moments of the ion and electron distribution functions.

Currently, only the radially-parabolic electric potential and the 
semi-Maxwellian electron distribution upstream are implemented, but the code 
is structured to allow easy extension to other cases.

## Installation

Installation requires simply that you 
[download Akiles2d](https://github.com/mariomerinomartinez/akiles2d/archive/master.zip) 
and add the base directory (the one that contains the `+akiles2d` directory) 
to your Matlab path.

### Dependencies

A recent version of Matlab is needed to run the code. 
Akiles2d has been developed in Matlab R2016b Academic version. 

Akiles2d depends on other Matlab packages that you can download from this
GitHub account: [logger](https://github.com/mariomerinomartinez/logger). These
packages must be installed and in your Matlab path to run Akiles2d.
 
## Quick usage guide

Akiles2d main function `akiles2d` is found under the `+akiles2d` directory.
This function performs a complete simulation, including the call to the 
preprocessor, the solver, and the postprocessor. All file writing 
and message logging operations are done in this function.

To run Akiles2d without any user inputs, simply add the base directory
to the Matlab path and run:

```Matlab
[data, solution] = akiles2d.akiles2d();
```

Akiles2d uses two structures to operate: 

* The `data` structure contains all the simulation parameters. It is generated
by the preprocessor, which reads a configuration file `simrc` given by the
user. The default `simrc` file, which can serve as a template for the user
files, is found under the `+akiles2d` directory.
* The `solution` structure stores the current solution during the iteration, 
and the final solution after it. All postprocessing functions add to this 
function, too.

The simulation is stored in a single directory, given by 
`data.akiles2d.simdir`. This is where the log file `log.txt` is stored.
The `data` structure is saved by the main function after being generated by 
the preprocessor to `data.mat`. 
Each `solution` structure of each iteration step is saved as a
separate file, named simply `#.mat`, where `#` is the iteration number.
The final iteration is saved to `final.mat`.
After postprocessing, the `solution` structure is also saved to `post.mat`.

### Code structure

Besides the `akiles2d` and `simrc` files described above, 
the code is structured into several Matlab subpackages as follows.
In the listing below, 
`+(potential model)`, `+(EVDF model)`, and `+(ions model)` are placeholders
for the name of the corresponding model. Presently, only
`+parabolic`, `+semimaxwellian`, and `+cold` are available, respectively.

* `+electrons`: The `moment` function that can be found in 
`+electrons\+(potential model)\+(EVDF model)` allows calculating any moment of the electron velocity distribution function, at any point. Some additional electron functions can be found in `+electrons\+(potential model)`.
* `+ions`: The `moment` function that can be found in 
`+ions\+(potential model)\+(ions model)` allows calculating any moment of the ion velocity distribution function.
* `+preprocessor`: Contains the `preprocessor` function, which facilitates the
preparation of the  `data` structure from multiple sources (default `simrc`
file, user `simrc`  file, and additional fields given through the console)
* `+solver`: Contains `solver` and `errorfcn`, two functions used in the 
iterative solution process by akiles2d.
* `+postprocessor`: functions intended to update the `solution` structure 
after converging to the self-consitent solutions are stored here.
* `+potential`: Some convenience functions for computing the potential `phi`
for each potential model.

## Documentation

A user/developer manual can be found in the `doc` subdirectory.  Any questions
about the operation or development of Akiles2d should be directed to [Mario
Merino](http://mariomerino.uc3m.es/).

### Testing

Unit tests are found in the `/test` subdirectory. After adding the package to
your Matlab path, you can run all tests by executing `runtests` from this 
subdirectory.

## Contributing

If you have any comments for improvement or 
are interested in contributing to the continued 
development of this or any of my other codes, you can contact us through 
[Mario's website](http://mariomerino.uc3m.es/).
 
## License

Copyright (c) 2017 Mario Merino and Javier Mauriño. 
The software is released as open source with the [MIT License](LICENSE.md).
