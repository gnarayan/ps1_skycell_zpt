Here is a basic summary of the data products currently available that could be useful for zeropoint checks/corrections.


SMF catalog (FITS table) -- full focal plane array (FPA) exposure catalog of detections with signal-to-noise>5, astrometric and zeropoint solution for full exposure. The catalog includes a MATCHED_REFS extension for the exposure-reference matched detections used for the astrometric and zeropoint solution, it looks to be for detections with signal-to-noise>20. Currently not distributed, but can easily be added to distribution.

-- MATCHED_REFS extension keywords that would be useful
* RA_REF, DEC_REF, MAG_INST, MAG_REF, COLOR_REF
* COLOR_REF is MAG_C1 -- g=g-i, r=r-i, i=r-i, z=i-z (if i recall correctly)


CMF warp catalog (FITS table) -- overlapping skycell region directly extracted from the SMF catalog and appears to be all sources now (not the signal-to-noise>20 mentioned before). Just to be clear, these detections are not measured on the warps directly. These are currently distributed.

-- keywords that could be useful
* RA_PSF, DEC_PSF, CAL_PSF_MAG, PSF_INST_MAG_SIG
* CAL_PSF_MAG_SIG should carry the calibration error like in the SMF, but it is 0 in the warp CMF for an unknown reason. SMF value is 0.0498 for this example (agrees with zpt_stdev in nightly report email)
* FLAGS, FLAGS2, PSF_QF_PERFECT, PSF_CHISQ, PSF_MAJOR/MINOR, PSF_FWHM_MAJ/MIN, MOMENTS*, KRON* can also be useful for rejecting junk and extended sources
* FLAGS, FLAGS2 bit descriptions can be found at
http://svn.pan-starrs.ifa.hawaii.edu/trac/ipp/browser/trunk/psModules/src/objects/pmSourceMasks.h?order=name


CMF diff catalog (FITS table) -- similar to the warp CMF with additional keywords related to diff stats. Have included in the example below.


Reference catalog -- getstar version -- full exposure FITS table catalog, extracted in same manner used for SMF calibration in IPP. No signal-to-noise limit other than the catalog itself mag~22, but may be source density limited depending on the field. The current reference catalog is from 2017 and the one used for DR2 (significance of issues near the pole are unclear).


Reference catalog -- dvo avextract alternative -- flat file for specific skycell (or could be full exposure field catalog) that is similar to getstar, but with magerr added and no cut on local density or any other parameters. It can be a bit tedious to make by skycell for all fields/skycells, so I will only do this if this catalog is wanted instead of a getstar catalog or making your own from DR2.


The simplest might be to use the SMF MATCHED_REFS catalog and look at the zeropoint around the target coordinates.

For more control over the matched sources, using a combination of the full SMF detections and either the dvo avextract alternative or your own DR2 reference catalog around targets or with the warp CMF and similar reference catalog covering a skycell could work.

As we have been trying to get zeropoints on chip/skycell level added for a while now, a future compromise might be to add the MATCHED_REFS extension from the SMF catalog that overlaps a particular skycell into the warp CMF and then into the WSDiff CMF catalog. That way there would always be a comparison reference set related to what was used for the full exposure zeropoint. This would still require some low level internal code modifications with testing and I cannot clearly predict the effort or timescale for that right now.


An example of the catalogs for PS19gzl with g_err~0.02 are attached to look at
https://star.pst.qub.ac.uk/sne/ps1yse/psdb/candidate/1092747511315806100/
09:27:47.51 +31:58:06.1

with the following files attached in a tarball

camera stage SMF -- o8812g0466o.1555325.cm.2210366.smf
warp stage CMF -- o8812g0466o.1555325.wrp.2185436.skycell.1961.093.cmf
WSdiff stage CMF -- RINGS.V3.skycell.1961.093.WS.dif.1862181.cmf
getstar version refcat -- o8812g0466o_g.getstar
dvo avextract alternative refcat -- yse.666_skycell.1961.093_ref170919v0_g.dat
