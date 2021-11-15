# Molecule: ha

A molecule scenario to run the role with a Highly Available and Distributed (HA/D) topology and a MySQL system data store.

This scenario is very memory intensive. The minimum memory on which this scenario has been succeeded is 32GB.

## Requirements

- Docker is installed
- Docker is configured to allow containers no less than 6GB RAM, 2GB swap, 4 CPUs. (default settings on macOS were inadequate; lower settings may be possible; the minimum was not searched for or found)
- molecule[docker] installed
- molecule[lint] is installed
- Appian installer bin is obtained and placed in the roles top level directory
- Appian k3.lic and k4.lic license files are obtained and placed in role's top level files directory
- The mysql connector jar is obtained and placed in the role's top level files directory.

## Usage

Please see the [molecule documentation](https://molecule.readthedocs.io/en/latest/getting-started.html) for more information.

To run this molecule scenario, issue the following from the role's top level directory:

    molecule converge

To run tests against this scenario, issue the following from the role's top level directory:

    molecule verify

To destroy anything built by this scenario, issue the following from the role's top level directory:

    molecule destroy
