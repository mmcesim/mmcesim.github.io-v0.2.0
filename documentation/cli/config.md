---
layout: default
title: Configuration
permalink: /doc/cli/config
parent: CLI Application
grand_parent: Documentation
nav_order: 2
has_children: false
---

# Configuration
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

***

Configuration file is in YAML format.
For more information about why YAML,
please refer to [this blog](https://blog.mmcesim.org/2022/07/file-format-yaml/).

***

## Version
This field (`version`) takes a string value representing the targeted mmCEsim version.
For compatibility convenience, this string can be used by the compiler
to decide the behaviour.
The current default value is the same as the compiler version (`0.1.0`).

***

## Meta
The `meta` is a map that provides metadata which can be used in report.
The used fields now include: `title`, `description`, `author`.

***

## Physics
The `physics` map contains physical system settings.

### Frequency
The bandwidth is specified in `frequency` filed.
Currently, this field does not do anything,
which is always assumed to be `narrow` (narrow band) now.

### Off Grid
This is actually about the model.
With the geometric channel model with grid,
there can be off-grid (or power leakage) problems.
Recently, there are also super resolution formulations to solve the problem.
But we still adopt the grid representation for its popularity and simplicity.
By setting `off_grid` field inside `physics` to `false`,
you may not consider the off grid effect.
The default value is `true`.

### Carriers
For a wideband system,
you may specify the number of carriers used in OFDM.
Its corresponding [macro](../alg/macro)s in [CALC](../alg/calc) is
`` `CARRIERS_NUM` ``.

***

## Nodes
The `nodes` is a sequence (array) of nodes in the channel network.
Transmitter (Tx), Receiver (Rx), Reconfigurable Intelligent Surface (RIS)
are all considered node (channels are the connecting edges to these nodes).
For each of its elements, you need to specify the following fields.

### ID
You need to specify a unique `id` for each node.
They are used in [`channels`](#channels) so that we know the direction of channel.

### Role
The `role` field has valid values `transmitter` (`Tx`), `receiver` (`Rx`),
and `RIS` (`IRS`).

### Num
The optional field `num` can be used to mean several copies of the same node
can be available for simplicity.
This is often used for multi-user scenarios.
Currently, this value is discarded, and the number is always 1.

### Size
The `size` means the antenna/element number for a node.
For Transmitters and receivers, it is the antenna number.
For RIS, it is the number of reflecting elements.
The value is a scalar for uniform linear array (ULA),
and is a 2-value array (for example `[8, 4]`) for uniform planar array (UPA).
For a 2-value array that has the second value set to 1,
it is still regarded as a ULA.

Its corresponding [macro](../alg/macro)s in [CALC](../alg/calc) are
`` `SIZE.T.x` ``, `` `SIZE.T.y` ``, `` `SIZE.T` ``,
`` `SIZE.R.x` ``, `` `SIZE.R.y` ``, `` `SIZE.R` ``, `` `SIZE.*` ``.

### Beam
The number of beams.
Dimensions similar to [Size](#size).

Its corresponding [macro](../alg/macro)s in [CALC](../alg/calc) are
`` `BEAM.T.x` ``, `` `BEAM.T.y` ``, `` `BEAM.T` ``,
`` `BEAM.R.x` ``, `` `BEAM.R.y` ``, `` `BEAM.R` ``, `` `BEAM.*` ``.

### Grid
The number of grids.
Dimensions similar to [Size](#size).
This is used in [CALC](../alg/calc) function [`\dictionary`](../alg/calc#dictionary)
and [macro](../alg/macro) [`` `DICTIONARY.T` ``](`../alg/macro#dictionaryt`) and [`` `DICTIONARY.R` ``](`../alg/macro#dictionaryr`).

Its corresponding [macro](../alg/macro)s in [CALC](../alg/calc) are
`` `GRID.T.x` ``, `` `GRID.T.y` ``, `` `GRID.T` ``,
`` `GRID.R.x` ``, `` `GRID.R.y` ``, `` `GRID.R` ``, `` `GRID.*` ``.

### Beamforming
In the `beamforming` field, the variable name `variable` is set,
and the beamforming scheme `scheme` is defined.
The `scheme` supports `random` and `custom`.
For a custom beamforming scheme, you also need to set the `formula` field with ALG language.

***

## Macro
User-defined macros are defined here.
Please refer to [documentation of macro](../alg/macro#user-defined-macros) for more information.

***

## Channels
The configuration `channels` is a sequence (array) of channel links.
The settings for each channel link is shown below.

### ID
Similar to [ID](#id) in [Nodes](#nodes),
each channel link has a unique identifier as in field `id`.

### From
The node which the link channel is transmitter from.
(`from`)

### To
The node which the link channel transmits to.
(`to`)

### Sparsity
The `sparsity` (number of clusters) of the channel.

### Gains
The channel `gains` consisting of pass loss.
There are two types of gains supported (`mode`):
`normal` and `uniform`.
For `normal` gains, `mean` and `variance` need to be specified.
For `uniform` gains, `min` and `max` need to be set.

***

## Sounding
Information related to the sounding procedure is defined here.

### Variables
Channel `variables` names are defined here.
- `received`: received signal vector
- `noise`: received noise vector
- `channel`: the cascaded channel

***

## Preamble
The `preamble` is code part before main simulation
(including sounding, estimation and report generation).

{: .tip }
Custom functions can be defined here.

This part is specified using the [ALG language](../alg).

***

## Estimation
The main code for `estimation`.
Function [`ESTIMATE`](../alg/function#estimate) is used here
to call the compressed sensing (CS) algorithms
either defined in the [standard ALG library](../alg/library)
or in the [preamble](#preamble).

{: .important-title }
> Basic MIMO Example
> 
> For a basic MIMO example,
> the estimation part is about converting the mmWave channel estimation
> to a compressed sensing problem before using [`ESTIMATE`](../alg/function#estimate),
> and recover the channel from the sparse result with [`RECOVER`](../alg/function#recover).

This part is specified using the [ALG language](../alg).

***

## Conclusion
In `conclusion`,
additional code after each [simulation job](#jobs)
is added here.

This part is specified using the [ALG language](../alg).

***

## Appendix
The code in `appendix` is added after all jobs are done.

This part is specified using the [ALG language](../alg).

***

## Simulation

### Backend
The `backend` includes `cpp` (C++ with Armadillo library),
`python` (Python with NumPy library)
and `matlab` or `octave`.
This sets the language it exports to and the backend simulation bases on.

### Jobs

### Report

#### Text Report

The simulation report can be exported as plain text report, for example
<script src="https://gist.github.com/Teddy-van-Jerry/0e181131baf2d60047b7ce6d24ee6422.js"></script>

#### LaTeX and PDF Report

LaTeX and its generated PDF report is also supported in addition to the
plain text `.rpt` report.

{: .hint-title }
> Example
> 
> [PDF Report Example](https://pub.mmcesim.org/mmCEsim_Example_Report.pdf)
> with [configuration example](https://pub.mmcesim.org/mmCEsim_Example_Config.pdf).
