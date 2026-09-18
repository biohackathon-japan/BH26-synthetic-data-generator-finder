# Critical review and revision record

## Assessment

The previous draft provided a useful outline but did not adequately describe the data model, distinguish collected features from features used in ranking, or quantify the evidence behind recommendations. It also described an older source revision and overstated the reach of optional narration. The revised paper is a hackathon software report with catalog and implementation evidence, not a validated recommendation study.

## Major issues addressed

1. **Incomplete Methods.** Added tables covering all 26 top-level method fields and all nested attributes, the complete dataset schema, controlled vocabularies, and the role of each feature. Separated filters, scores, explanations, and descriptive metadata. Maximum column count, language, update year, release information, and source links do not directly influence scores.
2. **Underspecified ranking.** Added exact score mappings, axis activation, purpose selection, tie-breakers, the effect of missing inputs, shortlist limits, and explanation thresholds. Explained that purpose is constant among purpose-matched candidates and that the four core inputs leave maturity as the only varying scored axis within this set.
3. **Thin Results.** Audited all rankable records, reported per-modality counts and metadata coverage, and retained three actually executed scenarios. Added machine-readable evidence in `paper/catalog-audit.json`. The audit does not certify source accuracy.
4. **Weak scientific positioning.** Added verified sources for synthpop, CTGAN, Synthea, and synthcity, and described SynFinder as a discovery workflow rather than a new generator. Avoided an unsupported claim of novelty over every existing discovery resource.
5. **Stale implementation description.** Updated to revision `89e0923fb1d5dcac5e676d28f207602f011ee47d`, including Pyodide-based static hosting, extra web candidates, source license, exports, and contribution mechanisms. Restricted the optional LLM description to the Streamlit path where it is actually connected.
6. **Evaluation overreach.** Reported 102 selected passing checks with clear exclusions. Explained that regression scenarios are not independent reference judgments and that mocked narration tests do not test live LLM factuality.
7. **Insufficient treatment of evidence gaps.** Added sparse scale evidence (3/66), the distinction between benchmark sizes and operating limits, optimistic treatment of unknown scale, uneven modality coverage, governance-documentation bias, and limited privacy annotations. Proposed specific independent evaluations without presenting them as completed work.

## Remaining substantive limitations

- Chang Sun’s name, affiliation, institutional email, and ORCID have been added using existing manuscripts and the Maastricht University profile (https://cris.maastrichtuniversity.nl/en/persons/chang-sun/). Roles reflect the development and writing described in this session. Additional contributors, final author order, funding, and acknowledgements remain to be confirmed.
- The team has not supplied a search strategy, inclusion criteria, extraction protocol, reviewer count, or annotation disagreement procedure. None has been invented.
- Catalog annotations are not independently source-audited. Having links, evidence text, or a valid year is not proof of correctness.
- No user study, independent expert benchmark, sensitivity analysis, generator-quality experiment, or deployment test is reported.
- A workflow figure or verified interface screenshot could improve presentation but is not required to interpret the current text.
- The manuscript license remains the template license; the MIT statement applies only to SynFinder source code.
- The manuscript has not been rendered to PDF or checked for the final publication page layout.

## Reproduction

Source repository: https://github.com/sunchang0124/SynFinder

Evaluated revision: `89e0923fb1d5dcac5e676d28f207602f011ee47d`.

Python: 3.11.15. Catalog loading uses the repository's Pydantic and PyYAML dependencies.

Run from this source revision:

```sh
PYTHONPATH=src python3 -m pytest -q \
  tests/test_catalog_integrity.py tests/test_golden_scenarios.py \
  tests/test_hard_filters.py tests/test_scoring.py \
  tests/test_dataset_matching.py tests/test_schema.py \
  tests/test_dataset_schema.py tests/test_catalog.py tests/test_intake.py \
  tests/test_explain.py tests/test_preview.py tests/test_report.py \
  tests/test_cli.py tests/test_contribute.py tests/test_llm.py
```

Observed result: **102 passed in 2.90 seconds**. Timing is environment-specific and is not a performance result.

For catalog statistics, load `load_catalog(Path('catalog'))` and use `generation_methods()` as the denominator. Count every value in `data_types`; these counts overlap. Count nonempty source-link fields and narrative lists separately. For scenarios, instantiate `Intake` from the exact objects in `paper/catalog-audit.json`, load `catalog/weights.yaml`, and call `rank(catalog.generation_methods(), intake, weights, top_n=3)`.

The source method catalog, taxonomy, and ranking rules did not change between the earlier evaluated revision and the new one; examples were nevertheless executed again at the new revision.

## Verified background sources

- synthpop: https://www.jstatsoft.org/article/view/v074i11
- CTGAN: https://arxiv.org/abs/1907.00503
- Synthea: https://pubmed.ncbi.nlm.nih.gov/29025144/
- synthcity: https://arxiv.org/abs/2301.07573
