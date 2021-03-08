# Radar Target Generation and Detection

## 2D CFAR

Instead of increasing the size of the CFAR output after the loop I initialized the output to be a zero matrix with the same size as RDM.

I then looped over the indices `[(1 + Tr + Gr):(size(RDM, 1) - Tr - Gr), (1 + Td + Gd):(size(RDM, 2) - Td - Gd)]` so that I wouldn't sample from elements outside the RDM array size.

I separates the training cells in 4 regions:
1. The first region have indices `[(i - Gr - Tr):(i + Gr + Tr), (j - Gd - Td):(j - Gd - 1)]`;
2. The second region have indices `[(i - Gr - Tr):(i + Gr + Tr), (j + Gd + 1):(j + Gd + Td)]`;
3. The third region have indices `[(i - Gr - Tr):(i - Gr - 1), (j - Gd):(j + Gd)]`;
4. The fourth region have indices `[(i + Gr + 1):(i + Gr + Tr), (j - Gd):(j + Gd)]`.
In this way I first extracted all these values and converted to values using `db2pow` and then calculated their mean. I finally added the offset and used the threshold to select the features.

To remove the false positives I played around with the training and guard cells dimension as well as the offset. It was mostly a trial and error process.

