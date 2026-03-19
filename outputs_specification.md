# Calculation utils output specification

## response

csv-file response_output.csv

energy (in MeV) column: energy.
calculated values columns: energy, fep, sep, dep, p511.
unc. columns: dfep, dsep, ddep, dp511. Uncertainty is relative in %.
normalized columns: FEP, SEP, DEP, P511
x-rays columns: nxep, exep, xep, dxep, XER, xept, dxept, XEPT
continum columns: a00-a58 (54columns), b00-b18(18columns)

## physspec

json-file with CalculationResults section

func -- full uncollided integral  N_i * A / N_yield  eff*A = n_i / I
fcol -- full collided integral
dfunc, dfcol -- relative unc. in %
y0 -- lines intensities (I_i * A)
x1 -- peak energies in MeV
y1 -- peak fluxes (= full count rate in detector)
dy1 -- abs. unc. of y1
x2 -- bin energies
y2 -- cr in energy bin


S_i = Eff_i * J_i, where J_i = I_i * A, so S_i = Eff_i * I_i * A, so S_i -- n_i 


StraightCalculationResults section -- straight MC in det calculation
energy -- in MeV
intensity
count_rate
efficiency = count_rate / intensity
defficiency -- rel. unc. (not in %)
spectr_energy -- energy bin
spectr_cr -- spectr cr


## appspec

tsv-file

energy -- energy in MeV
efficiency
defficiency -- abs. efficiency
count_rate -- peak crs
intensity -- peak intensity: I * N

