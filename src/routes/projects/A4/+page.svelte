<script>
	import { base } from '$app/paths';
</script>

<svelte:head>
	<title>A4: Persuasive or Deceptive Visualization</title>
</svelte:head>

<header class="report-header">
	<p class="report-label">Assignment 4 · Vis &amp; Society</p>
	<h1>A4: Persuasive or Deceptive Visualization</h1>
	<p class="report-byline">Milin Tunsiricharoengul</p>
</header>

<main class="report">

	<section class="report-section report-meta">
		<p><strong>Dataset:</strong> World Bank Data by Indicator</p>
		<p><strong>Proposition:</strong> Higher gender equality is associated with stronger economies.</p>
	</section>

	<!-- ─── For Proposition ─── -->
	<section class="report-section">
		<h2>For Proposition</h2>

		<figure>
			<img class="viz-image" src="{base}/images/a4-for.png" alt="Visualization arguing for the proposition: higher gender equality is associated with stronger economies" />
		</figure>

		<ol class="report-questions">
			<li>
				Filtering out low quality data: <span class="score">−1.5</span>
				<ul>
					<li>
						Although data quality is a real issue, I ultimately filtered out countries missing
						25% or more years of data in order to remove most countries that didn't follow
						the upward trend. Earlier filters by population and specific outliers (Iran,
						Madagascar, Rwanda, Uzbekistan) excluded well-known small countries like
						Denmark, Singapore, and Norway, so I shifted to the data quality filter to better
						disguise this approach as earnest and necessary. It's important to note that this
						certainly leaves out important nuances around countries in active conflict, limited
						infrastructure, large informal economies, etc.
					</li>
				</ul>
			</li>
			<li>
				Displaying dots in addition to trend lines: <span class="score">2</span>
				<ul>
					<li>
						I chose to show the color-coded dots to help the reader understand the data's
						true structure: each dot represents a single country in a given year. This also
						strengthens confidence in the trend lines. Without the dots, a reader could
						suspect that a trend line was drawn through only two points. The multitude of
						dots confirms that each trend line reflects a genuine, data heavy pattern.
					</li>
				</ul>
			</li>
			<li>
				Titles: <span class="score">0.5</span>
				<ul>
					<li>
						The chart title is more deceptive than the axis titles. It implies causation where
						the data only shows correlation. For readers with a predisposition to the idea of
						feminism driving economic growth, the title alone can become the takeaway
						without further question. The axis titles are simplified for readability, but also take
						a stance on how the measured value should be interpreted. "Economic
						prosperity" is an abstract concept that can be measured in many ways, but the
						visualization conflates it entirely with GDP per capita PPP.
					</li>
				</ul>
			</li>
			<li>
				Blue trend lines: <span class="score">1.5</span>
				<ul>
					<li>
						The clearest way to show a positive correlation between female education and
						economic strength was through upward trend lines: one per country, reinforced
						by a single bold black overall trend line. I chose a blue, green, and black palette
						because this visualization targets an audience more likely to take it seriously in
						traditionally "masculine" colors. I initially had a pink and purple version that more
						immediately signaled the subject matter, but risked alienating the audience that
						the visualization aims to persuade. I chose seriousness over the cognitive
						dissonance of masculine colors on a feminist subject.
					</li>
				</ul>
			</li>
			<li>
				Not sizing by population: <span class="score">−0.5</span>
				<ul>
					<li>
						Initially I encoded dot size by population, as large countries like China, India, and
						the US fell clearly on the trend line. However, filtering out countries with missing
						data excluded China and India, leaving the US as the largest dot. This looked
						suspicious, so I switched to uniform dot sizes to avoid drawing attention to which
						countries were driving the trend. Since everything was an upward trend by this
						point, sizing by population was redundant.
					</li>
				</ul>
			</li>
		</ol>
	</section>

	<!-- ─── Against Proposition ─── -->
	<section class="report-section">
		<h2>Against Proposition</h2>

		<p class="gif-notice">&#9654; This is a GIF — let it play</p>
		<figure>
			<img class="viz-image" src="{base}/images/a4-counter.gif" alt="Visualization arguing against the proposition: higher gender equality is associated with stronger economies" />
		</figure>

		<ol class="report-questions">
			<li>
				Faceting out different indications of gender equality: <span class="score">2</span>
				<ul>
					<li>
						Displaying three panels across education, life expectancy, and labor force
						participation gives the reader a comprehensive picture and signals that no
						indicators are being cherry-picked.
					</li>
				</ul>
			</li>
			<li>
				Case study examples: <span class="score">1.5</span>
				<ul>
					<li>
						The country walkthroughs strengthen credibility by showing that major countries
						across all regions exhibit the expected behavior, and that regardless of gender
						parity levels, GDP remains consistent. Since I selected these countries myself,
						this isn't fully earnest. Many countries could contradict this pattern. For what most
						countries exhibit, however, it is a strong and palatable representation.
					</li>
				</ul>
			</li>
			<li>
				Shorten education axis: <span class="score">−0.5</span>
				<ul>
					<li>
						I shortened the "% Enrolled in Higher Education" axis to cap at 100% although its
						values extend beyond this range. Initially, I did this to avoid confusion, but found
						that this stretched the x-axis in a way that emphasized a red cluster that signified
						high female enrollment was irrelevant to GDP. This glaring red cluster became an
						important annotation that supported the takeaway message.
					</li>
				</ul>
			</li>
			<li>
				Annotations: <span class="score">2</span>
				<ul>
					<li>
						The annotations are essential for guiding the reader through a visually dense
						chart. By explicitly pointing out the red cluster and dense purple regions, the
						reader is led to conclude independently that gender equality and economic
						strength are not associated. This makes the takeaway self-discovered rather than
						imposed.
					</li>
				</ul>
			</li>
		</ol>
	</section>

	<!-- ─── Reflection ─── -->
	<section class="report-section">
		<h2>Reflection</h2>
		<p>
			This exercise made "ethical analysis" really difficult to define. I realized that creating a
			visualization with a message in mind makes true neutrality impossible. It surprised me
			how easily decisions that furthered my message could be disguised as earnest ones,
			slipping under a reader's perception. The most acceptable choices are those made in
			the name of clarity and exploration. When data contradicts the message, the honest
			response is to explain it transparently, caveat the conclusion, or revise it entirely. Data
			visualization is meant to make data comprehensible, but in practice I see it largely used
			to persuade. The line I'd draw between persuasive and misleading choices is that
			persuasive choices make something genuinely true about the data more visible, while
			misleading choices manufacture the appearance of a pattern that the data doesn't
			confidently support. This could be through selective exclusion, axis manipulation,
			oversimplification, or burying contradicting data points.
		</p>
	</section>

</main>

<style>
	.report-meta {
		display: flex;
		flex-direction: column;
		gap: 0.25rem;
		padding: 1rem 1.25rem;
		background: oklch(97% 0.5% 200);
		border-left: 3px solid var(--color-accent);
		border-radius: 0 4px 4px 0;
		font-size: 0.95rem;
	}

	.report-meta p {
		margin: 0;
		color: oklch(30% 2% 200);
	}

	.score {
		font-weight: 700;
		color: var(--color-accent);
	}

	.viz-image {
		width: 100%;
		max-width: 900px;
		display: block;
		margin: 0 auto;
	}

	.gif-notice {
		font-weight: 700;
		color: #c0392b;
		font-size: 0.9rem;
		margin-bottom: 0.5rem;
		letter-spacing: 0.03em;
	}
</style>
