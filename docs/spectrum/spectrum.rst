===================
The Spectrum object
===================

The :class:`~radis.spectrum.spectrum.Spectrum` class in RADIS primarily 
holds the result from a :class:`~radis.lbl.factory.SpectrumFactory` calculation. 
It can be  to plot different quantities a posteriori, or manipulate output units
(for instance convert a spectral radiance per wavelength units to a spectral 
radiance per wavenumber). Spectrum objects can be combined along the line of sight 
with the :func:`~radis.los.slabs.MergeSlabs` and :func:`~radis.los.slabs.SerialSlabs` methods. 
Spectrum objects can be quickly compared with the :func:`~radis.spectrum.compare.plot_diff` 
function. Finally, they can be stored in JSON files and in :class:`~radis.tools.database.SpecDatabase`.

However, a :class:`~radis.spectrum.spectrum.Spectrum` object can also be 
generated from text files, or numpy arrays, or directly from other radiation
codes such as `SPECAIR <http://www.specair-radiation.net/>`_ . This allows to 
immediatly have access to all the above functions, methods and features to any 
experimental spectrum, or calculated spectrum from different radiation software, 
or combination of them. 

Features are more explicitely presented in the :doc:`howto` document.   


Spectral quantities
-------------------

A :class:`~radis.spectrum.spectrum.Spectrum` object is built on top of a dictionary structure, and can handle 
quantities with any name. However, for the conversion (units) and update methods 
to work properly it is recommended to follow the naming conventions below: 

Variables convoluted with an instrumental slit function: 

- **radiance**: spectral radiance after self-absorption (typically in mW/cm2/sr/nm)
- **transmittance**: spectral transmittance (0-1)
- **emissivity**: spectral emissivity (0-1)

Variables not convoluted: 

- **radiance_noslit**: spectral radiance after self-absorption (typically in mW/cm2/sr/nm)
- **transmittance_noslit**: spectral transmittance (0-1)
- **emissivity_noslit**: spectral emissivity (0-1)
- **emisscoeff**: spectral emission density (typically in mW/cm3/sr/nm)
- **absorbance**: spectral absorbance, or optical depth (no dimension)
- **abscoeff**: spectral absorption coefficient (typically in cm-1)
- **abscoeff_continuum**: a pseudo-continuum generated by RADIS :class:`~radis.lbl.factory.SpectrumFactory` 

Most of the quantities above can be recomputed from one another. In an homogeneous
slab, one requires an emission spectral density, and an absorption spectral density, 
to be able to recompute the other quantities (provided that conditions such as path length
are given). Under equilibrium, only one quantity is needed. Missing quantities
can be recomputed automatically with the :meth:`~radis.spectrum.spectrum.Spectrum.update` 
method. 


Units
-----

Units are stored in the :attr:`~radis.spectrum.spectrum.Spectrum.units` dictionary 

It is strongly advised not to modify the dictionary above. However, spectral quantities 
can be retrieved in arbitrary units with the :meth:`~radis.spectrum.spectrum.Spectrum.get` 
method.

When an instrument slit function is convoluted with :meth:`~radis.spectrum.spectrum.Spectrum.apply_slit`,
the unit of the convolved quantities may change, depending on how the slit function 
was normalised. Several options are available in RADIS. Please refer to the documentation 
of the :meth:`~radis.spectrum.spectrum.Spectrum.apply_slit` method. 
