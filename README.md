# heatmap
draw a heatmap with R

library(gplots)  #  Import the gplots package
diff.data <- read.delim("~/diff.data.txt", row.names=1)  # Read files for heat map plotting
hr <- hclust(as.dist(1-cor(t(diff.data))), method = "complete")  # Cluster the probes (row)
hc <- hclust(as.dist(1-cor(diff.data)), method = "complete")  # Cluster the samples (column)
mycl <- cutree(hr, 2)  # Divide the probes to two groups according to the clustering tree (up-regulation and down-regulation)
mycolhr <- rainbow(2, start=0.3, end=0.8)  # Assign colors for the two groups of probes (left)
mycolhr <- mycolhr[as.vector(mycl)]  # Match the colors with the probe groups
pdf("heatmap-final9.pdf", height = 12, width = 10)  # Start plotting (in PDF format); set width and height
heatmap.2(as.matrix(diff.data),      # Convert the data to “matrix” format (required for heatmap.2 function)
          Rowv=as.dendrogram(hr),    # Define the sequence of rows according to the previous clustering result
          Colv=as.dendrogram(hc),    # Define the sequence of columns according to the previous clustering result
          col=greenred(900),         # Assign colors for heat map blocks
          scale = "row",             # Standardize the map based on the rows
          density.info="none",       # Set the color key and remove the density lines
          trace="none",              # Set the heat map and remove the trace lines
          RowSideColors=mycolhr)     # Set the probe colors for different groups
dev.off()  # Finish plotting
