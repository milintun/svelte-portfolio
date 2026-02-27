<script>
	import { base } from '$app/paths';
</script>

<svelte:head>
	<title>A3: Visualization Critique &amp; Redesign</title>
</svelte:head>

<nav>
	<a href="{base}/">Home</a>
	<a class="current" href="{base}/projects">Projects</a>
	<a href="{base}/contact">Contact</a>
	<a href="{base}/resume">Resume</a>
	<a href="https://github.com/milintun" target="_blank">Github</a>
</nav>

<header class="report-header">
	<p class="report-label">Assignment 3 · Vis &amp; Society</p>
	<h1>A3: Visualization Critique &amp; Redesign</h1>
	<p class="report-byline">Milin Tunsiricharoengul</p>
</header>

<main class="report">

	<!-- ─── Read Visualization ─── -->
	<section class="report-section">
		<h2>Read Visualization</h2>
		<ul class="report-questions">
			<li>
				Polar expanding spiral that plots time clockwise from 2020 to present day (January 2022)
				<ul>
					<li>Month at every clockhand hour position</li>
					<li>Width of shaded area linearly correlates with number of new cases that week</li>
					<li>
						Number of cases starts at 0, increases to peak in 2021, dies down and almost
						disappears before July 2021, then grows again to smaller peak in September 2021,
						and finally increases to absolute maximum in January 2022
						<ul>
							<li>Minimum cases approaching summer time</li>
							<li>Grows and peaks during winter</li>
							<li>Current case growth is going to provoke a peak larger than all previous</li>
						</ul>
					</li>
				</ul>
			</li>
		</ul>
	</section>

	<!-- ─── Critique Visualization ─── -->
	<section class="report-section">
		<h2>Critique Visualization</h2>
		<p>
			After spending a few minutes breaking down this visualization, I came to appreciate how the
			visualization allows for relatively easy comparisons between weekly case quantities and was
			able to fit 2 years of time plotting concisely due to its circular form. This polar axis,
			however, is a double-edged sword. While condensing lots of data into a neat, compact box,
			the spiral is an unintuitive method of visualizing time, which I confirmed when many of my
			non-data-viz friends struggled to interpret the graph. I found it thoughtful and clever that
			the 12 month ticks lined up with all rings of the spiral, and that they were analogous to
			the familiar 12 hour ticks on a clock. Even so, these month labels cannot make up for
			another major point of confusion caused by the spiral design: the illusions of depth and
			warping. Because the spiral begins with a smaller radius and spins out, I naturally assigned
			it a sense of depth, as if looking down a spiral staircase. I could imagine the center being
			"farther away" from me, muddying comparisons between areas at different "depths." Also due
			to the spiral form, time stretches toward the outer rings. Although this could be an
			intentional choice to prioritize detail in the more recent months, it's a disorienting
			departure from our linear, regular perception of time increments.
		</p>
		<p>
			Despite the atypical time axis, the other attributes of the visualization were well fitted to
			the context and spiral. The shaded area is reflected across the axis, allowing its width
			between two points to be easily compared no matter what orientation the points on the spiral
			are in. This symmetry was another double-edged sword, though. There was no immediate sense of
			directionality. Because the "start" of the axis was hidden in the center and the area colored
			both sides of the line, it isn't intuitive where to begin looking and what direction to go
			forward in. Some simple channels were working well for me: The color encoding is red, which I
			perceived to be threatening or negative, which in the case of symbolizing Covid-19 cases, was
			accurate. There was no serious overplotting. We only need to know the number of cases
			throughout time to understand that the current amount of cases is concerningly large. The
			peaks and troughs are also easy to spot due to the width operating above the perceptual
			threshold of the just noticeable difference. A small note I had on the width, however, was
			that the legend maxed out well before even the 2nd largest peak's width. The legend can
			extend to encompass the largest value and contain multiple labels for absolute and relative
			peaks so that we can search for the case size at every point of interest.
		</p>
	</section>

	<!-- ─── Sketches ─── -->
	<section class="report-section">
		<h2>Sketches</h2>

		<section class="report-subsection">
			<h3>Sketch 1 — Address: Depth Illusion, Time Warp, Incomplete Legend</h3>
			<figure>
				<img src="{base}/images/a3-1.jpg" alt="Sketch 1: flattened circular axis addressing depth illusion and time warp" />
			</figure>
			<div class="report-decisions-grid">
				<div class="report-decisions-col">
					<h4>Design Decisions</h4>
					<ul>
						<li>
							To address the depth illusion and time warp, I flattened the spiral to be a single
							2D circle axis. In order to combine the 2 years onto a 1 year time axis, I encoded
							each year with a different color and hatching direction.
						</li>
						<li>
							I kept the month labels and ticks since they were still compatible with this
							visualization and they solidify the circular year representation comprehension.
						</li>
						<li>
							I also preserved the symmetric area reflected across the line because my time was
							still plotted as a circular form.
						</li>
						<li>
							I shaded the 2022 area with only the 2022 encoding, so it really only shades the
							regions where 2022 saw more cases than 2021. I chose to only shade the overlap
							because the most important takeaway is that we're experiencing more cases in January
							2022 than we did in January 2021, which was the peak.
						</li>
					</ul>
				</div>
				<div class="report-decisions-col">
					<h4>Reflection</h4>
					<ul>
						<li>
							The coinciding areas placed on the same axis makes comparison between cases during
							the same time of the year trivial.
						</li>
						<li>
							The tradeoff of the overlapping areas is that comparing cases across different times
							between the years becomes difficult because of the density of information at one
							point in time.
						</li>
						<li>
							The issue of directionality is exacerbated. With the closed circle, I have no idea
							where to start, go, and end.
						</li>
						<li>
							The legend is more informative because it encompasses all widths. It might be
							helpful to annotate the number of cases at the peak in January 2021 so we can
							compare the 2 peak quantities.
						</li>
					</ul>
				</div>
			</div>
		</section>

		<section class="report-subsection">
			<h3>Sketch 2 — Address: Directionality, Unintuitive Time Axis</h3>
			<figure>
				<img src="{base}/images/a3-2.JPG" alt="Sketch 2: small multiples line chart of 2020 vs 2021" />
			</figure>
			<div class="report-decisions-grid">
				<div class="report-decisions-col">
					<h4>Design Decisions</h4>
					<ul>
						<li>
							I stripped the data down to a continuous time dimension and number of cases
							dimension. I wanted to observe what I would lose and gain in understanding by
							plotting these the most intuitive way: a small multiples line chart of 2020 vs 2021.
						</li>
					</ul>
				</div>
				<div class="report-decisions-col">
					<h4>Reflection</h4>
					<ul>
						<li>The linear, left to right time axis is immediately recognizable and readable.</li>
						<li>
							I can still compare between months of different years easily. The line heights or
							areas are easy to distinguish when they're vertically oriented and directly on top of
							each other rather than on different points of a circular path.
						</li>
						<li>
							One important thing we lost is continuity between 2020 and 2021. With polar time,
							we were able to coincide months of the two years while also plotting them on the
							same line. We must choose lining up months in this visualization.
						</li>
						<li>
							Because the circle was condensed, we also lose detail when laying 2 charts out
							linearly. The axis representing a year becomes much shorter, and our plotted lines
							become more like broad strokes than fine detail. This is acceptable since we're
							focused on larger comparisons between peaks.
						</li>
					</ul>
				</div>
			</div>
		</section>

		<section class="report-subsection">
			<h3>Sketch 3 — Address: Space Efficiency, Continuity</h3>
			<figure>
				<img src="{base}/images/a3-3.JPG" alt="Sketch 3: tighter spiral preserving space efficiency and continuity" />
			</figure>
			<div class="report-decisions-grid">
				<div class="report-decisions-col">
					<h4>Design Decisions</h4>
					<ul>
						<li>
							Iterating on the first sketch, I funnily iterated kind of back to the original
							visualization with this sketch by turning the single circle into a tighter spiral. I
							figured that this could preserve the space efficiency and continuity of the polar
							axis, achieve simple month-by-month comparisons between years, and if the spiral was
							tight enough, avoid the time and depth warp.
						</li>
						<li>
							I encoded a sense of direction with a start point at the inner top ring and end arrow
							at the outer top ring of the spiral, as well as a color gradient that develops
							clockwise along the axis.
						</li>
						<li>
							The spiral is one-sided because it contains the full 2 years of information, but the
							case data just switches sides at the end of the first year.
						</li>
					</ul>
				</div>
				<div class="report-decisions-col">
					<h4>Reflection</h4>
					<ul>
						<li>
							The color gradient should be sequential rather than divergent since we're encoding a
							start and end time, not deviation from the middle time.
						</li>
						<li>
							The tightness of the spiral makes this read as a circle, and combined with the
							plotting flipping to include the second year, this visualization is even harder to
							understand than the original.
						</li>
						<li>
							It doesn't seem like we gained more than we lost in terms of clarity and
							intuitiveness.
						</li>
					</ul>
				</div>
			</div>
		</section>

		<section class="report-subsection">
			<h3>Sketch 4 — Address: Intuitiveness</h3>
			<figure>
				<img src="{base}/images/a3-4.JPG" alt="Sketch 4: discretized monthly heatmap grid with season color coding" />
			</figure>
			<div class="report-decisions-grid">
				<div class="report-decisions-col">
					<h4>Design Decisions</h4>
					<ul>
						<li>
							I couldn't leave my sketches off with my previous sketch, so I tried to create a
							visualization that was a more extreme departure from the polar, area chart that I
							kept making. I realized that there's no reason the data needs to be continuous. My
							takeaway was always to compare between aggregates of cases across whole months and
							seasons. I decided to discretize the data into months.
						</li>
						<li>
							I thought heavily about how we see months. I perceive the months of the year as 4
							rows of 3 month blocks, but attempted to group them roughly by 3 month color-coded
							seasons in this visualization so that patterns can be observed across seasons.
						</li>
						<li>
							Since I used color to frame the seasons, I used shading to encode the magnitude of
							cases, which makes peaks and troughs immediately stand out.
						</li>
					</ul>
				</div>
				<div class="report-decisions-col">
					<h4>Reflection</h4>
					<ul>
						<li>
							Shading works well in the context of this visualization because it's unnecessary to
							make small comparisons between medium shades. We're mostly curious to find that we
							are currently entering the darkest square, which is darker than our previous darkest
							square.
						</li>
						<li>
							Shading as an encoding channel also invokes a sense of urgency, and the evolution of
							empty to dotted to hatch feels relevant to contagion and the spread of illness.
						</li>
						<li>
							The discretization and shading legend simplify things in a very compact and
							intuitive way.
						</li>
						<li>
							The season framing is confusing, and doesn't seem necessary. The grouping by
							seasons also adds an arbitrary layer of organization that isn't necessary.
						</li>
					</ul>
				</div>
			</div>
		</section>
	</section>

	<!-- ─── Final Design ─── -->
	<section class="report-section">
		<h2>Final Design</h2>
		<figure>
			<img src="{base}/images/a3-final-design.png" alt="Final redesign: discretized monthly heatmap of Covid-19 cases" />
		</figure>

		<section class="report-subsection">
			<h3>Design Decisions</h3>
			<p>
				As of January 6, 2022, weekly new Covid-19 cases were significantly higher than they were
				at the peak of the pandemic in early 2021. It was thus expected that January 2022 would
				reach a new peak in the number of Covid-19 cases. This is the primary message my
				visualization communicates.
			</p>
			<p>
				To convey this clearly, I plotted one mark for each month to discretize years of daily
				data into a common time unit that is both broad enough to keep the table simple and
				granular enough to capture the highs and lows of the pandemic intensity. Additionally, I
				designed a slight left-to-right skew in the marks to subtly encode the direction of time
				from January to December. The consistent left-to-right and fitted nature of the marks
				enables viewers to start from the top left and read left-to-right, then top-to-bottom.
				The marks are shaded with a red color scheme to invoke a sense of urgency and negativity
				associated with Covid-19 cases. I applied a power transformation (exponent of 0.7) to the
				weekly confirmed case counts to compress extreme values, preserving visibility of the
				absolute max while enhancing comparability among lower-magnitude observations. For the
				same reason, I encoded the marks with a stepped color scale instead of a continuous
				gradient to create perceptually distinct intervals. This ensured that the perceived
				differences exceed just noticeable differences.
			</p>
			<p>
				Finally, to communicate that it was the projection of cases in January 2022 that was
				concerningly high based on how the month began rather than past data, I only partially
				plotted that month and labeled it to make note of the predictive data. I included subtle
				empty marks for the remainder of 2022 to allow the reader to imagine how past trends
				could extrapolate and evolve (lows in the summer, an even stronger peak in the winter,
				etc.).
			</p>
		</section>

		<section class="report-subsection">
			<h3>Reflection</h3>
			<p>
				Through my iterative sketching, I was able to uncover tradeoffs between all potential
				structures and whittle away at where the true emphasis of the visualization lied. In the
				fourth sketch, I recognized that discretization was essential to achieving simplicity,
				intuitiveness, and compactness. As a result, I adopted discrete monthly marks and
				introduced a subtle warp to encode directionality within that shape. In the second sketch,
				I identified a tradeoff between enabling month-to-month comparison and preserving visual
				continuity across years. Because the primary message revolves around comparison rather
				than temporal continuity, I prioritized comparability, accepting reduced emphasis on the
				connection between the end of 2020 and the beginning of 2021. I also confronted a
				tradeoff in my color encoding strategy. A linear scale would have aligned with reader
				expectations, but it dampened perceptual differentiation because extreme peaks compressed
				variation among lower values. The power scale improved perceptual sensitivity across the
				distribution, allowing meaningful differences away from the max to remain visible.
				Although this introduced nonlinearity, I attempted to mitigate potential confusion by
				using a stepped color legend rather than a continuous gradient. The discrete steps
				clarified key value ranges explicitly, making the nonlinear scaling interpretable while
				preserving perceptual comparability.
			</p>
		</section>
	</section>

</main>

<style>
	.report-decisions-grid {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 1.5rem;
		margin-top: 1rem;
	}

	.report-decisions-col h4 {
		font-size: 0.85rem;
		font-weight: 700;
		text-transform: uppercase;
		letter-spacing: 0.06em;
		color: var(--color-accent);
		margin: 0 0 0.5rem;
	}

	.report-decisions-col ul,
	.report-decisions-col ol {
		padding-left: 1.25rem;
		line-height: 1.7;
		color: oklch(20% 2% 200);
	}

	.report-decisions-col li {
		margin-bottom: 0.5rem;
	}

	@media (max-width: 640px) {
		.report-decisions-grid {
			grid-template-columns: 1fr;
		}
	}
</style>
