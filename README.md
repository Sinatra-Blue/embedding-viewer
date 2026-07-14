# Marine Acoustic Embeddings — Interactive 3D UMAP

An interactive 3D visualisation of self-supervised acoustic embeddings
learned from marine passive acoustic monitoring (PAM) data. Part of a PhD
project at the University of Exeter developing foundation-model and
self-supervised learning approaches for scalable marine ecological
monitoring.

## Live viewer

**[View the interactive 3D embedding](https://sinatra-blue.github.io/marine-acoustic-embeddings/)**

Rotate, zoom, and hover over individual points. Toggle between two colour
modes using the buttons above the plot:

- **Colour by cluster** — HDBSCAN cluster assignments in the embedding
  space, showing the fine-grained acoustic structure the model has
  discovered without any labels.
- **Colour by recorder** — segments coloured by which of the two VAALCO
  offshore hydrophones (VAALCO01 or VAALCO02) they came from. The two
  colours are mixed throughout the space, showing that recorder-hardware
  differences have been effectively removed.

## About the data

The viewer shows a 30,000-segment subsample of a larger corpus of
approximately 1.2 million two-second segments extracted from raw 96 kHz
passive acoustic recordings collected in Gabonese offshore waters.

Segments were embedded into a 256-dimensional space using a
self-supervised hierarchical encoder (five-layer 1D convolutional
front-end followed by a four-layer transformer aggregator), trained
with a temporal contrastive objective on unlabelled data. The embeddings
were then projected to three dimensions via UMAP for visualisation.

## Methodological context

Marine passive acoustic monitoring datasets are typically collected by
hydrophones whose slowly-varying hardware characteristics can dominate
the learned representation in a standard contrastive-learning pipeline.
Early results in this project showed this failure mode directly: the two
Gabonese hydrophones formed two spatially separated regions of the
embedding space, and 136 clusters at their boundaries could be identified
as recorder-specific artefacts at harmonic fractions of the sampling rate.

To address this, this work introduces **RecorderInvariantNorm**, a
depthwise convolutional exponential-moving-average approximation of
per-channel energy normalisation, applied between the raw waveform input
and the convolutional encoder. Re-training with this component in place
unified the two recorder bodies, eliminated the artefact clusters
entirely, and produced the biologically-coherent embedding space shown
in this viewer.

## Related software

The recordings underlying this analysis were originally in the proprietary
Wildlife Acoustics WAC format. To make them accessible without commercial
software, this project also contributes **pywac** — the first
pure-Python decoder and converter for WAC files, ported from the R
`bioacoustics` C++ Rcpp implementation.

- **pywac:** [github.com/Sinatra-Blue/pywac](https://github.com/Sinatra-Blue/pywac)

## Author

Sophie Wilday, PhD researcher, University of Exeter.
Supervisors: Sareh Rowlands, Matt Witt, Lucy Hawkes.

## Citation

If this viewer or the underlying methods are useful to your work, please
cite the associated thesis (in preparation, 2028) or contact the author.
