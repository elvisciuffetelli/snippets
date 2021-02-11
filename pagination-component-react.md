
###### A React pagination component


```jsx
import React, { useEffect, useState } from 'react';
import PropTypes from 'prop-types';
import Icon from './Icon';

const Pagination = ({ currentPageIndex, totalRecords, pageLimit, pageNeighbours, onPageChanged, className }) => {
  // const [currentPage, setCurrentPage] = useState(1);
  const [pages, setPages] = useState();
  const [totalPages, setTotalPages] = useState(Math.ceil(totalRecords ? totalRecords / pageLimit : 0));
  const LEFT_PAGE = 'LEFT';
  const RIGHT_PAGE = 'RIGHT';
  const HIDDEN_PAGE = '...';

  /**
   * Helper method for creating a range of numbers
   * range(1, 5) => [1, 2, 3, 4, 5]
   */
  const range = (from, to, step = 1) => {
    let i = from;
    const range = [];

    while (i <= to) {
      range.push(i);
      i += step;
    }

    return range;
  };

  const gotoPage = page => {
    const currentPage = Math.max(0, Math.min(page - 1, totalPages - 1));

    // setCurrentPage(currentPage);
    onPageChanged(currentPage);
  };

  const handleClick = page => {
    gotoPage(page);
  };

  const handleMoveLeft = evt => {
    evt.preventDefault();
    gotoPage(currentPageIndex);
  };

  const handleMoveRight = evt => {
    evt.preventDefault();
    gotoPage(currentPageIndex + 2);
  };

  /**
   * Let's say we have 10 pages and we set pageNeighbours to 2
   * Given that the current page is 6
   * The pagination control will look like the following:
   *
   * (1) < {4 5} [6] {7 8} > (10)
   *
   * (x) => terminal pages: first and last page(always visible)
   * [x] => represents current page
   * {...x} => represents page neighbours
   */
  const fetchPageNumbers = () => {
    /**
     * totalNumbers: the total page numbers to show on the control
     * totalBlocks: totalNumbers + 2 to cover for the left(<) and right(>) controls
     */
    const totalNumbers = pageNeighbours * 2 + 3;
    const totalBlocks = totalNumbers + 2;

    if (totalPages > totalBlocks) {
      const startPage = Math.max(2, currentPageIndex + 1 - pageNeighbours);
      const endPage = Math.min(totalPages - 1, currentPageIndex + 1 + pageNeighbours);

      let pages = range(startPage, endPage);

      /**
       * hasLeftSpill: has hidden pages to the left
       * hasRightSpill: has hidden pages to the right
       * spillOffset: number of hidden pages either to the left or to the right
       */

      const hasLeftSpill = startPage > 2;
      const hasRightSpill = totalPages - endPage > 1;
      const spillOffset = totalNumbers - (pages.length + 1);

      switch (true) {
        // handle: (1) < {5 6} [7] {8 9} (10)
        case hasLeftSpill && !hasRightSpill:
          // const extraPages = range(startPage - spillOffset, startPage - 1);
          pages = [LEFT_PAGE, 1, HIDDEN_PAGE, ...range(startPage - spillOffset, startPage - 1), ...pages, totalPages];
          break;

        // handle: (1) {2 3} [4] {5 6} > (10)
        case !hasLeftSpill && hasRightSpill:
          // const extraPages = range(endPage + 1, endPage + spillOffset);
          pages = [1, ...pages, ...range(endPage + 1, endPage + spillOffset), HIDDEN_PAGE, totalPages, RIGHT_PAGE];
          break;

        // handle: (1) < {4 5} [6] {7 8} > (10)
        case hasLeftSpill && hasRightSpill:
        default:
          pages = [LEFT_PAGE, 1, HIDDEN_PAGE, ...pages, HIDDEN_PAGE, totalPages, RIGHT_PAGE];
          break;
      }

      return [...pages];
    }

    return range(1, totalPages);
  };

  useEffect(
    () => {
      if (totalRecords && totalRecords > 0) {
        setTotalPages(Math.ceil(totalRecords / pageLimit));
      }
    },
    // eslint-disable-next-line react-hooks/exhaustive-deps
    [totalRecords, pageLimit]
  );

  useEffect(
    () => {
      if (totalPages > 0) {
        const pages = fetchPageNumbers();
        setPages(pages);
      }
    },
    // eslint-disable-next-line react-hooks/exhaustive-deps
    [totalPages, currentPageIndex]
  );

  if (!totalRecords || totalPages === 1) return null;

  return (
    <>
      <nav className={`pagination ${className}`}>
        {pages &&
          pages.map((page, index) => {
            if (page === LEFT_PAGE)
              return (
                <button key={index} type="button" className="pagination__prev" onClick={handleMoveLeft}>
                  <Icon name="circle-arrow-left" />
                </button>
              );

            if (page === RIGHT_PAGE)
              return (
                <button key={index} type="button" className="pagination__next" onClick={handleMoveRight}>
                  <Icon name="circle-arrow-right" />
                </button>
              );
            if (page === HIDDEN_PAGE) return <span key={index}>...</span>;

            return (
              <button
                key={index}
                type="button"
                className={currentPageIndex + 1 === page ? 'active' : ''}
                onClick={() => handleClick(page)}
              >
                {page}
              </button>
            );
          })}
      </nav>
    </>
  );
};

Pagination.propTypes = {
  // totalRecords: PropTypes.number.isRequired,
  pageLimit: PropTypes.number,
  pageNeighbours: PropTypes.number,
  onPageChanged: PropTypes.func
};

export default Pagination;
```