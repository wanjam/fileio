# Splitting a 16-bit trigger into two 8-bit trigger channels

Our lab uses a Biosemi Active two system. Theoretically, this system is capable of reading 24bit triggers. However, we only send 8bit triggers from a PC, and additional 8bit triggers from our monitor (a Viewpixx /EEG). That is, pins 1-8 are PC triggers and pins 9-13 are the second device. However, the resulting .bdf file contains a single(!) event-channel with both kinds of triggers clashed together to one 16bit channel. This folder contains a modified version of EEGlab's 'pop_fileio.m' and FieldTrip's 'fileio' plugin that splits the triggers before storing them in the event structure.

## Usage
- pop_fileio calls ft_read_event. This function depends on a lot on `/private` functions that come with the fileio-plugin. It therefore does not suffice to simply shadow pop_fileio.m & ft_read_event.m with the modified versions. Instead, these modified versions need to be located at exactly the same location as the original ones relative to the fileio plugin folder.
- If you only have a single 8bit trigger-channel in your file, this function will still provide the correct output.

### Option A: Only use this modified version
The simplest solution is to just replace 'pop_fileio.m' and 'ft_read_event.m' in your eeglab folder with the modified ones from this repository. Obviously, you then cannot use the faster original version anymore.

### Option B: Create an optional copy

Just clone this repo and add it to your path AFTER(!) you called eeglab, so the whole fileio plugin-folder will be shadowed by the modified version. e.g.,:

```matlab
eeglab;
close(findobj('tag', 'EEGLAB')); % 'nogui' doesn't load plugins...
addpath(genpath('PATH/TO/THIS/MOD')));
```

Note: This modified version is intended for usage in our lab. We did not write the modifications with maximum compatibility in mind. So use at your own risk.

%  This program is free software: you can redistribute it and/or modify
%  it under the terms of the GNU General Public License as published by
%  the Free Software Foundation, either version 3 of the License, or
%  (at your option) any later version.
%
%  This program is distributed in the hope that it will be useful,
%  but WITHOUT ANY WARRANTY; without even the implied warranty of
%  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
%  GNU General Public License for more details.
%
%  You should have received a copy of the GNU General Public License
%  along with this program. If not, see <http://www.gnu.org/licenses/>.

# original fileio readme
-------------------------------------------------------------------------
This is the FieldTrip FILEIO module. It contains functions for
reading EEG and MEG data from a large variety of files. Furthermore,
it llows reading additional information from files, such as sensor
positions, anatomical MRIs, triangulated meshes, etc.

For more information please visit http://www.fieldtriptoolbox.org/development/modules

The source code for zlib is available at http://www.zlib.net

-------------------------------------------------------------------------------
Copyright (C) 2008-2017, Donders Institute for Brain, Cognition and Behaviour, Radboud University, The Netherlands (DCCN, DCC, DCN)
Copyright (C) 2012-2017, Max Planck Institute for Psycholinguistics, The Netherlands (MPI)
Copyright (C) 2008-2017, The Wellcome Trust Centre for Neuroimaging, University College London, UK (UCL)
Copyright (C) 2010-2013, Swammerdam Institute for Life Sciences, University of Amsterdam (SILS)
Copyright (C) 2008-2009, Centre for Cognitive Neuroimaging in Glasgow, United Kingdom (CCNi)
Copyright (C) 2009-2009, Netherlands Institute for Neuroscience (NIN)
Copyright (C) 2003-2008, F.C. Donders Centre, Radboud University Nijmegen, The Netherlands (FCDC)
Copyright (C) 2004-2007, Nijmegen Institute for Cognition and Information, The Netherlands (NICI)
Copyright (C) 2004-2005, Universitatsklinikum Hamburg-Eppendorf, Germany (UKE)
Copyright (C) 2003-2004, Center for Sensory Motor Interaction, University Aalborg, Denmark (SMI)
Copyright (C) 1999-2003, Department of Medical Physics, Radboud University Nijmegen, The Netherlands (MBFYS)

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/
-------------------------------------------------------------------------------

