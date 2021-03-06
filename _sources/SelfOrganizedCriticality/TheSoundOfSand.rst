..  Copyright (C)  Jan Pearce
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.

.. _SOC_8:

The Sound of Sand
-----------------

As the implementation of SandPile runs, it records the number of cells that topple during each time step, accumulating the results in a list called ``toppled_seq``. After running the model in :ref:`Section 10.5 <SOC_5>`, we can extract the resulting signal:

::

    signal = pile2.toppled_seq

To compute the power spectrum of this signal we can use the SciPy function ``welch``:

::

    from scipy.signal import welch

    nperseg = 2048
    freqs, spectrum = welch(signal, nperseg=nperseg, fs=nperseg)

This function uses Welch’s method, which splits the signal into segments and computes the power spectrum of each segment. The result is typically noisy, so Welch’s method averages across segments to estimate the average power at each frequency.

The parameter ``nperseg`` specifies the number of time steps per segment. With longer segments, we can estimate the power for more frequencies. With shorter segments, we get better estimates for each frequency. The value, 2048, seems to balances these tradeoffs.

The parameter ``fs`` is the “sampling frequency”, which is the number of data points in the signal per unit of time. By setting ``fs=nperseg``, we get a range of frequencies from ``0`` to ``nperseg/2``. This range is convenient, but because the units of time in the model are arbitrary, it doesn’t mean much.

The return values, ``freqs`` and ``powers``, are NumPy arrays containing the frequencies of the components and their corresponding powers, which we can plot. :ref:`Figure 10.6 <SOC_FIG_6>` shows the result.

.. _SOC_FIG_6:

.. figure:: Figures/POWER_SPECT.png
    :align: center
    :alt: "Figure Figure 10.6: Power spectrum of the number of toppled cells over time, log-log scale."

    Figure 10.6: Power spectrum of the number of toppled cells over time, log-log scale.


For frequencies between 10 and 1000 (in arbitrary units), the spectrum falls on a straight line, which is what we expect for pink or red noise.

The gray line in the figure has slope −1.58, which indicates that :math:`logP(f) ∼ −β logf` with parameter :math:`β=1.58`, which is the same parameter reported by Bak, Tang, and Wiesenfeld. This result confirms that the sand pile model generates pink noise.
