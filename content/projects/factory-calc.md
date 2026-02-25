+++
title = "Factory Calculator"
weight = 1

[extra]
summary = "A Rust CLI tool that calculates optimal production rates for factory simulation games like Factorio and Satisfactory."
tags = ["Rust", "CLI", "Game Tools"]
repo = "https://github.com/pmanahan/factory_calc"
+++

A Rust-based calculator for factory games (Factorio, Satisfactory, Techtonica, Foundry). It abstracts common factory mechanics — items, recipes, production rates — into reusable components exposed through a CLI.

Built with a workspace layout: core model library with zero external dependencies, and a CLI binary using `clap` for argument parsing and `serde` for JSON output.
