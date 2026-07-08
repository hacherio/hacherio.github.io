--- 
title: Biohub - Cell tracking Computer Vision Model
date: 2026-06-29 06:00:00 -0400
categories: [Projects, Models]
tags: [machine-learning]
---

## Biohub Cell Tracking Introduction
Starting with a hefty kaggle competition, the [Biohub - Cell Tracking During Development](https://www.kaggle.com/competitions/biohub-cell-tracking-during-development/overview) competition is a biomedical computer vision problem run under a Kaggle-style competition format. The goal here is to detect and track cells across frames in 3D microscopy videos. Labeled answers are rather limited or incomplete, so only a subset of cells are annotated in each video. These annotations are stored in a GEFF format, which are file formats used to track life sciences of cell/or organelle.

Algorithm task to do is:
1. Detect where each cell is in every frame using x, y, z coordinates
2. Edge accuracy - Track and link which cell in frame 1 became which cell in frame 2, etc..
3. Detect divisons - figure out when one cell splits into two daughter cells.
Altogether creates a family tree of all cell showing who came from whom.

Basically, what this competition evaluates is the tracking metric of edge accuracy (how well cells linked across time) and division detection (how well mitosis events identifies). These two metrics comes with a scoring of:
$score = adjusted\_edge\_jaccard + 0.1 \cdot division\_jaccard$

- Edge Jaccard - with how correct your links between cells across time (matching to the ground truth positions allowing small error)

- Division Jaccard - correctly spot the moment cells split into two

Both are combined into one score. If this works well, it removes the tedious labor for biologists and study the progression without hand-tracking cells one frame at a time.

## The Datasets
- Input: .zarr file volume containing single numpy-like array with shaped as (Time, Z, Y, X) in uint16 format, similarly a stack of 3D images over 100 timepoints
- Ground truth (training only): .geff files, which are annotation graph format containing: 
  - nodes/ids - node ID array 
  - nodes/props/{t,z,y,x}/values - integer centroid coordinates per node
  - edge/ids - edge array of shape (N, 2) with cols (source_id, target_id) 

These tells where nodes (cells) are and edges (links between cell and next frame/daughter cells). One thing we should note is that the annotations are sparse, where not all cells are labeled, so the algorithm we are building will have to work with incomplete data answers during training.

## File directories
- train/ - training samples, where each sample has paired .zarr (image vol) and .geff (annotated tracking graph)
- test/ - example test samples copied from train, which only contains .zarr image volumes only. No ground truth is given.
- sample_submission.csv - submission file to show correct format

## Model algorithm 
