# Responsive Angular D3js Graph

Create your graph component

### `component.html`
```
<div class="chart"></div>
```

### `component.ts`
```
export class ChartComponent implements OnInit, OnChanges {

  @Input()
  data: ChartDatum[] = [];

  @Input()
  height: string = 200;

  private svg;
  private graphContainer;
  private width: number;
  private height: number;

  @HostListener('window:resize', ['$event'])
  onResize() {
    this.reloadChart();
  }

  constructor(private container: ElementRef) {
  }

  ngOnInit() {
    setTimeout(() => {
      this.initializeSvg();
      this.generateGraphFromData();
    });
  }

  ngOnChanges(): void {
    if ( this.contentContainer ) {
      this.generateGraphFromData();
    }
  }

  private initializeSvg() {
    const chartElement = this.container.nativeElement.querySelector('.chart');
    const wrapper = select(chartElement);

    const widthWrapper = chartElement.clientWidth;
    const heightWrapper = this.height;
    const margin = {
      top: 10,
      right: 10,
      bottom: 10,
      left: 10
    };

    this.width = widthWrapper - margin.left - margin.right;
    this.height = heightWrapper - margin.top - margin.bottom;

    this.svg = wrapper.append('svg')
      .attr('width', widthWrapper + margin.left + margin.right)
      .attr('height', this.height + margin.top + margin.bottom);

    const svgContainer = this.svg.append('g')
      .attr('transform', `translate(${margin.left},${margin.top})`);

    this.graphContainer = svgContainer.append('g')
      .attr('class', 'graph-container');
  }

  private generateGraphFromData() {
     // Do What ever your chart need to do inside `this.graphContainer`
  }

  private reloadChart() {
    const wrapper = select(this.container.nativeElement.querySelector('.chart'));
    wrapper.select('svg').remove();
    this.initializeSvg();
    this.generateGraphFromData();
  }
}

```

### Explanation

> ```
> @HostListener('window:resize', ['$event'])
> onResize() {
>   this.reloadChart();
> }
> ```

Each time the window is reduced or increased, `reloadChart` is called. It delete the `svg` and its chidren and recreate the chart.
